---
title: Defragmentace metriky v Azure Service Fabric | Microsoft Docs
description: "Přehled použití defragmentace nebo balení jako strategie pro metriky v Service Fabric"
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: e5ebfae5-c8f7-4d6c-9173-3e22a9730552
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: b253cc07066092aa82d218c9c82c8aac502245a8
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="defragmentation-of-metrics-and-load-in-service-fabric"></a><span data-ttu-id="b2b46-103">Defragmentace metriky a zatížení v Service Fabric</span><span class="sxs-lookup"><span data-stu-id="b2b46-103">Defragmentation of metrics and load in Service Fabric</span></span>
<span data-ttu-id="b2b46-104">Správce služby Fabric clusteru prostředků výchozí strategii správy metriky zatížení v clusteru je distribuce zatížení.</span><span class="sxs-lookup"><span data-stu-id="b2b46-104">The Service Fabric Cluster Resource Manager's default strategy for managing load metrics in the cluster is to distribute the load.</span></span> <span data-ttu-id="b2b46-105">Zajištění, že uzly jsou rovnoměrně využité zabraňuje úrovněmi horkého a studeného body, které vést k kolizí i nevyužité prostředky.</span><span class="sxs-lookup"><span data-stu-id="b2b46-105">Ensuring that nodes are evenly utilized avoids hot and cold spots that lead to both contention and wasted resources.</span></span> <span data-ttu-id="b2b46-106">Distribuce zatížení v clusteru je také nejbezpečnější z hlediska zbývajících selhání, protože zajišťuje selhání nebude vyjměte vysoké procento daného zatížení.</span><span class="sxs-lookup"><span data-stu-id="b2b46-106">Distributing workloads in the cluster is also the safest in terms of surviving failures since it ensures that a failure doesn’t take out a large percentage of a given workload.</span></span> 

<span data-ttu-id="b2b46-107">Správce prostředků clusteru Service Fabric podporuje různé strategie pro správu zatížení, která je defragmentace.</span><span class="sxs-lookup"><span data-stu-id="b2b46-107">The Service Fabric Cluster Resource Manager does support a different strategy for managing load, which is defragmentation.</span></span> <span data-ttu-id="b2b46-108">Defragmentace znamená, že místo došlo k pokusu o distribuci využití metriky napříč clusterem, ho jsou konsolidovány.</span><span class="sxs-lookup"><span data-stu-id="b2b46-108">Defragmentation means that instead of trying to distribute the utilization of a metric across the cluster, it is consolidated.</span></span> <span data-ttu-id="b2b46-109">Konsolidace je právě inverzi výchozí vyrovnávání strategie – místo minimalizovat průměrná směrodatnou odchylku metriky zatížení, správce prostředků clusteru pokusí o zvýšit ji.</span><span class="sxs-lookup"><span data-stu-id="b2b46-109">Consolidation is just an inversion of the default balancing strategy – instead of minimizing the average standard deviation of metric load, the Cluster Resource Manager tries to increase it.</span></span>

## <a name="when-to-use-defragmentation"></a><span data-ttu-id="b2b46-110">Kdy použít Defragmentace</span><span class="sxs-lookup"><span data-stu-id="b2b46-110">When to use defragmentation</span></span>
<span data-ttu-id="b2b46-111">Distribuce zatížení v clusteru spotřebuje některé prostředky na každém uzlu.</span><span class="sxs-lookup"><span data-stu-id="b2b46-111">Distributing load in the cluster consumes some of the resources on each node.</span></span> <span data-ttu-id="b2b46-112">Některé úlohy vytvoření služby, které jsou velmi velké a využívat většinu nebo všechny uzlu.</span><span class="sxs-lookup"><span data-stu-id="b2b46-112">Some workloads create services that are exceptionally large and consume most or all of a node.</span></span> <span data-ttu-id="b2b46-113">V těchto případech je možné, že po velkých úloh získávání vytvořen, který není dostatek místa v každém uzlu, abyste je mohli spustit.</span><span class="sxs-lookup"><span data-stu-id="b2b46-113">In these cases, it's possible that when there are large workloads getting created that there isn't enough space on any node to run them.</span></span> <span data-ttu-id="b2b46-114">Rozsáhlejší úlohy nejsou potíže s Service Fabric; v těchto případech správce prostředků clusteru určí, že potřebuje reorganizace cluster tak, aby uvolnil prostor pro velké pracovního vytížení.</span><span class="sxs-lookup"><span data-stu-id="b2b46-114">Large workloads aren't a problem in Service Fabric; in these cases the Cluster Resource Manager determines that it needs to reorganize the cluster to make room for this large workload.</span></span> <span data-ttu-id="b2b46-115">Do té doby této úlohy má však má systém čekat před naplánována v clusteru.</span><span class="sxs-lookup"><span data-stu-id="b2b46-115">However, in the meantime that workload has to wait to be scheduled in the cluster.</span></span>

<span data-ttu-id="b2b46-116">Pokud máte mnoho služeb a stavu pohyb, může to trvat dlouhou dobu pro velké zatížení umístit do clusteru.</span><span class="sxs-lookup"><span data-stu-id="b2b46-116">If there are many services and state to move around, then it could take a long time for the large workload to be placed in the cluster.</span></span> <span data-ttu-id="b2b46-117">Toto je pravděpodobnější, pokud dalších zatížení v clusteru jsou velké a proto trvat delší dobu reorganizovat.</span><span class="sxs-lookup"><span data-stu-id="b2b46-117">This is more likely if other workloads in the cluster are also large and so take longer to reorganize.</span></span> <span data-ttu-id="b2b46-118">Tým Service Fabric měří času vytvoření v simulace tohoto scénáře.</span><span class="sxs-lookup"><span data-stu-id="b2b46-118">The Service Fabric team measured creation times in simulations of this scenario.</span></span> <span data-ttu-id="b2b46-119">Zjistili jsme, že trvalo déle, mnohem vytváření velké služeb, při využití clusteru získali výše 30 až 50 %.</span><span class="sxs-lookup"><span data-stu-id="b2b46-119">We found that creating large services took much longer as soon as cluster utilization got above between 30% and 50%.</span></span> <span data-ttu-id="b2b46-120">Do tohoto scénáře zavedli jsme defragmentace jako strategie vyrovnávání.</span><span class="sxs-lookup"><span data-stu-id="b2b46-120">To handle this scenario, we introduced defragmentation as a balancing strategy.</span></span> <span data-ttu-id="b2b46-121">Zjistili jsme, že pro větší zatížení, zejména ty, kde je důležité, defragmentace skutečně pomohly tyto nové úlohy vytváření získat naplánován v clusteru.</span><span class="sxs-lookup"><span data-stu-id="b2b46-121">We found that for large workloads, especially ones where creation time was important, defragmentation really helped those new workloads get scheduled in the cluster.</span></span>

<span data-ttu-id="b2b46-122">Můžete nakonfigurovat defragmentace metriky tak, aby měl správce prostředků clusteru pokusí proaktivně zúžit zatížení služeb do menšího počtu uzlů.</span><span class="sxs-lookup"><span data-stu-id="b2b46-122">You can configure defragmentation metrics to have the Cluster Resource Manager to proactively try to condense the load of the services into fewer nodes.</span></span> <span data-ttu-id="b2b46-123">To pomáhá zajistit, že je téměř vždy prostor pro velké služeb bez reorganizace clusteru.</span><span class="sxs-lookup"><span data-stu-id="b2b46-123">This helps ensure that there is almost always room for large services without reorganizing the cluster.</span></span> <span data-ttu-id="b2b46-124">Nemusí reorganizovat clusteru umožňuje vytváření rozsáhlých úloh s rychle.</span><span class="sxs-lookup"><span data-stu-id="b2b46-124">Not having to reorganize the cluster allows creating large workloads quickly.</span></span>

<span data-ttu-id="b2b46-125">Většina lidí nepotřebují defragmentace.</span><span class="sxs-lookup"><span data-stu-id="b2b46-125">Most people don’t need defragmentation.</span></span> <span data-ttu-id="b2b46-126">Služby jsou obvykle být malé, takže není vyhledání místnosti pro ně v clusteru.</span><span class="sxs-lookup"><span data-stu-id="b2b46-126">Services are usually be small, so it’s not hard to find room for them in the cluster.</span></span> <span data-ttu-id="b2b46-127">Když reorganizaci je možné, přejde rychle znovu protože většina služeb jsou malé a přesunout rychle a paralelně.</span><span class="sxs-lookup"><span data-stu-id="b2b46-127">When reorganization is possible, it goes quickly, again because most services are small and can be moved quickly and in parallel.</span></span> <span data-ttu-id="b2b46-128">Pokud máte velké služby a je vytvořen rychle pak strategie defragmentace potřebovat, ale je pro vás.</span><span class="sxs-lookup"><span data-stu-id="b2b46-128">However, if you have large services and need them created quickly then the defragmentation strategy is for you.</span></span> <span data-ttu-id="b2b46-129">Probereme kompromisy defragmentace další použití.</span><span class="sxs-lookup"><span data-stu-id="b2b46-129">We'll discuss the tradeoffs of using defragmentation next.</span></span> 

## <a name="defragmentation-tradeoffs"></a><span data-ttu-id="b2b46-130">Defragmentace kompromisy</span><span class="sxs-lookup"><span data-stu-id="b2b46-130">Defragmentation tradeoffs</span></span>
<span data-ttu-id="b2b46-131">Defragmentace může zvýšit impactfulness chyb, protože běží další služby na uzlech, které nesplní.</span><span class="sxs-lookup"><span data-stu-id="b2b46-131">Defragmentation can increase impactfulness of failures, since more services are running on nodes that fail.</span></span> <span data-ttu-id="b2b46-132">Defragmentace můžete taky zvýšit náklady, protože zdrojů v clusteru, musí uchovávat v rezervy, čeká na vytvoření rozsáhlejší úlohy.</span><span class="sxs-lookup"><span data-stu-id="b2b46-132">Defragmentation can also increase costs, since resources in the cluster must be held in reserve, waiting for the creation of large workloads.</span></span>

<span data-ttu-id="b2b46-133">Následující obrázek poskytuje vizuální reprezentace dva clustery, ten, který je defragmentovat a ten, který není.</span><span class="sxs-lookup"><span data-stu-id="b2b46-133">The following diagram gives a visual representation of two clusters, one that is defragmented and one that is not.</span></span> 

<span data-ttu-id="b2b46-134"><center>
![Porovnání vyrovnáváním a defragmentovat clustery][Image1]
</center></span><span class="sxs-lookup"><span data-stu-id="b2b46-134"><center>
![Comparing Balanced and Defragmented Clusters][Image1]
</center></span></span>

<span data-ttu-id="b2b46-135">V případě vyrovnáváním zvažte počet pohybů, které by bylo nutné umístit mezi největší objekty služby.</span><span class="sxs-lookup"><span data-stu-id="b2b46-135">In the balanced case, consider the number of movements that would be necessary to place one of the largest service objects.</span></span> <span data-ttu-id="b2b46-136">V defragmentované clusteru může být umístěny na uzly čtyř nebo pěti velké zatížení bez čekání pro jiné služby pro přesun.</span><span class="sxs-lookup"><span data-stu-id="b2b46-136">In the defragmented cluster, the large workload could be placed on nodes four or five without having to wait for any other services to move.</span></span>

## <a name="defragmentation-pros-and-cons"></a><span data-ttu-id="b2b46-137">Defragmentace výhody a nevýhody</span><span class="sxs-lookup"><span data-stu-id="b2b46-137">Defragmentation pros and cons</span></span>
<span data-ttu-id="b2b46-138">Jaké jsou proto tyto koncepční kompromisy?</span><span class="sxs-lookup"><span data-stu-id="b2b46-138">So what are those other conceptual tradeoffs?</span></span> <span data-ttu-id="b2b46-139">Tady je rychlý tabulku dbát:</span><span class="sxs-lookup"><span data-stu-id="b2b46-139">Here’s a quick table of things to think about:</span></span>

| <span data-ttu-id="b2b46-140">Defragmentace specialisté</span><span class="sxs-lookup"><span data-stu-id="b2b46-140">Defragmentation Pros</span></span> | <span data-ttu-id="b2b46-141">Cons Defragmentace</span><span class="sxs-lookup"><span data-stu-id="b2b46-141">Defragmentation Cons</span></span> |
| --- | --- |
| <span data-ttu-id="b2b46-142">Umožňuje rychlejší vytváření velké služeb</span><span class="sxs-lookup"><span data-stu-id="b2b46-142">Allows faster creation of large services</span></span> |<span data-ttu-id="b2b46-143">Koncentráty načíst do menšího počtu uzlů, zvýšení kolizí</span><span class="sxs-lookup"><span data-stu-id="b2b46-143">Concentrates load onto fewer nodes, increasing contention</span></span> |
| <span data-ttu-id="b2b46-144">Umožňuje snížit přesun dat během vytváření</span><span class="sxs-lookup"><span data-stu-id="b2b46-144">Enables lower data movement during creation</span></span> |<span data-ttu-id="b2b46-145">Selhání může mít vliv na další služby a způsobit další změny</span><span class="sxs-lookup"><span data-stu-id="b2b46-145">Failures can impact more services and cause more churn</span></span> |
| <span data-ttu-id="b2b46-146">Umožňuje bohaté popis požadavků a recyklace místa</span><span class="sxs-lookup"><span data-stu-id="b2b46-146">Allows rich description of requirements and reclamation of space</span></span> |<span data-ttu-id="b2b46-147">Složitější celkové konfiguraci správy prostředků</span><span class="sxs-lookup"><span data-stu-id="b2b46-147">More complex overall Resource Management configuration</span></span> |

<span data-ttu-id="b2b46-148">Je možné kombinovat defragmentované a normální metriky ve stejném clusteru.</span><span class="sxs-lookup"><span data-stu-id="b2b46-148">You can mix defragmented and normal metrics in the same cluster.</span></span> <span data-ttu-id="b2b46-149">Správce prostředků clusteru pokusí o konsolidovat co nejvíce při šíření na jiné defragmentace metriky.</span><span class="sxs-lookup"><span data-stu-id="b2b46-149">The Cluster Resource Manager tries to consolidate the defragmentation metrics as much as possible while spreading out the others.</span></span> <span data-ttu-id="b2b46-150">Výsledky kombinování defragmentace a vyrovnávání strategie závisí na několika různými faktory, včetně:</span><span class="sxs-lookup"><span data-stu-id="b2b46-150">The results of mixing defragmentation and balancing strategies depends on several factors, including:</span></span>
  - <span data-ttu-id="b2b46-151">počet vyrovnávání metriky oproti počet defragmentace metriky</span><span class="sxs-lookup"><span data-stu-id="b2b46-151">the number of balancing metrics vs. the number of defragmentation metrics</span></span>
  - <span data-ttu-id="b2b46-152">Tom, zda všechny služby používá oba typy metriky</span><span class="sxs-lookup"><span data-stu-id="b2b46-152">Whether any service uses both types of metrics</span></span> 
  - <span data-ttu-id="b2b46-153">metriky vah</span><span class="sxs-lookup"><span data-stu-id="b2b46-153">the metric weights</span></span>
  - <span data-ttu-id="b2b46-154">načte aktuální metrika</span><span class="sxs-lookup"><span data-stu-id="b2b46-154">current metric loads</span></span>
  
<span data-ttu-id="b2b46-155">Experimentování, je potřeba určit přesnou konfiguraci nezbytné.</span><span class="sxs-lookup"><span data-stu-id="b2b46-155">Experimentation is required to determine the exact configuration necessary.</span></span> <span data-ttu-id="b2b46-156">Před povolením defragmentace metriky v produkčním prostředí doporučujeme důkladné měření vašich úloh.</span><span class="sxs-lookup"><span data-stu-id="b2b46-156">We recommend thorough measurement of your workloads before you enable defragmentation metrics in production.</span></span> <span data-ttu-id="b2b46-157">To platí hlavně při kombinování defragmentace a vyrovnáváním metriky v rámci stejné služby.</span><span class="sxs-lookup"><span data-stu-id="b2b46-157">This is especially true when mixing defragmentation and balanced metrics within the same service.</span></span> 

## <a name="configuring-defragmentation-metrics"></a><span data-ttu-id="b2b46-158">Konfigurace defragmentace metriky</span><span class="sxs-lookup"><span data-stu-id="b2b46-158">Configuring defragmentation metrics</span></span>
<span data-ttu-id="b2b46-159">Konfigurace metriky defragmentace je globální rozhodnutí v clusteru a jednotlivé metriky můžete vybrat pro defragmentaci.</span><span class="sxs-lookup"><span data-stu-id="b2b46-159">Configuring defragmentation metrics is a global decision in the cluster, and individual metrics can be selected for defragmentation.</span></span> <span data-ttu-id="b2b46-160">Následující konfigurace fragmenty kódu ukazují, jak nakonfigurovat metriky pro defragmentaci.</span><span class="sxs-lookup"><span data-stu-id="b2b46-160">The following config snippets show how to configure metrics for defragmentation.</span></span> <span data-ttu-id="b2b46-161">V tomto případě "Metric1" je nakonfigurovaná jako defragmentace metriku, zatímco "Metric2" bude dále vyváženy normálně.</span><span class="sxs-lookup"><span data-stu-id="b2b46-161">In this case, "Metric1" is configured as a defragmentation metric, while "Metric2" will continue to be balanced normally.</span></span> 

<span data-ttu-id="b2b46-162">ClusterManifest.xml:</span><span class="sxs-lookup"><span data-stu-id="b2b46-162">ClusterManifest.xml:</span></span>

```xml
<Section Name="DefragmentationMetrics">
    <Parameter Name="Metric1" Value="true" />
    <Parameter Name="Metric2" Value="false" />
</Section>
```

<span data-ttu-id="b2b46-163">pomocí souboru ClusterConfig.json pro samostatné nasazení nebo Template.json pro Azure hostované clustery:</span><span class="sxs-lookup"><span data-stu-id="b2b46-163">via ClusterConfig.json for Standalone deployments or Template.json for Azure hosted clusters:</span></span>

```json
"fabricSettings": [
  {
    "name": "DefragmentationMetrics",
    "parameters": [
      {
          "name": "Metric1",
          "value": "true"
      },
      {
          "name": "Metric2",
          "value": "false"
      }
    ]
  }
]
```


## <a name="next-steps"></a><span data-ttu-id="b2b46-164">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b2b46-164">Next steps</span></span>
- <span data-ttu-id="b2b46-165">Správce prostředků clusteru má man možnosti popisující clusteru.</span><span class="sxs-lookup"><span data-stu-id="b2b46-165">The Cluster Resource Manager has man options for describing the cluster.</span></span> <span data-ttu-id="b2b46-166">Další informace o nich, projděte si tento článek na [popisující cluster Service Fabric](service-fabric-cluster-resource-manager-cluster-description.md)</span><span class="sxs-lookup"><span data-stu-id="b2b46-166">To find out more about them, check out this article on [describing a Service Fabric cluster](service-fabric-cluster-resource-manager-cluster-description.md)</span></span>
- <span data-ttu-id="b2b46-167">Metriky se, jak správce prostředků služby Fabric clusteru spravuje využívání a kapacity v clusteru.</span><span class="sxs-lookup"><span data-stu-id="b2b46-167">Metrics are how the Service Fabric Cluster Resource Manger manages consumption and capacity in the cluster.</span></span> <span data-ttu-id="b2b46-168">Další informace o metriky a způsob jejich konfigurace, podívejte se na [v tomto článku](service-fabric-cluster-resource-manager-metrics.md)</span><span class="sxs-lookup"><span data-stu-id="b2b46-168">To learn more about metrics and how to configure them, check out [this article](service-fabric-cluster-resource-manager-metrics.md)</span></span>

[Image1]:./media/service-fabric-cluster-resource-manager-defragmentation-metrics/balancing-defrag-compared.png
