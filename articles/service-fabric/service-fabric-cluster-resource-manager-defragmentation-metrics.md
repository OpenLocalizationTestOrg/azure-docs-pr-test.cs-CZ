---
title: aaaDefragmentation metriky v Azure Service Fabric | Microsoft Docs
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
ms.openlocfilehash: d09045a6cf196d2771f1a0794637f4579d3eb96b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="defragmentation-of-metrics-and-load-in-service-fabric"></a><span data-ttu-id="98778-103">Defragmentace metriky a zatížení v Service Fabric</span><span class="sxs-lookup"><span data-stu-id="98778-103">Defragmentation of metrics and load in Service Fabric</span></span>
<span data-ttu-id="98778-104">strategie výchozí Hello portálu Service Fabric clusteru Resource Manager pro správu metriky zatížení v clusteru hello je toodistribute hello zatížení.</span><span class="sxs-lookup"><span data-stu-id="98778-104">hello Service Fabric Cluster Resource Manager's default strategy for managing load metrics in hello cluster is toodistribute hello load.</span></span> <span data-ttu-id="98778-105">Zajištění, že uzly jsou rovnoměrně využité zabraňuje úrovněmi horkého a studeného body, které vést tooboth kolizí a nevyužité prostředky.</span><span class="sxs-lookup"><span data-stu-id="98778-105">Ensuring that nodes are evenly utilized avoids hot and cold spots that lead tooboth contention and wasted resources.</span></span> <span data-ttu-id="98778-106">Distribuce zatížení v clusteru hello je také hello nejbezpečnější z hlediska zbývajících selhání, protože zajišťuje selhání nebude vyjměte vysoké procento daného zatížení.</span><span class="sxs-lookup"><span data-stu-id="98778-106">Distributing workloads in hello cluster is also hello safest in terms of surviving failures since it ensures that a failure doesn’t take out a large percentage of a given workload.</span></span> 

<span data-ttu-id="98778-107">Hello správce prostředků clusteru Service Fabric podporuje různé strategie pro správu zatížení, která je defragmentace.</span><span class="sxs-lookup"><span data-stu-id="98778-107">hello Service Fabric Cluster Resource Manager does support a different strategy for managing load, which is defragmentation.</span></span> <span data-ttu-id="98778-108">Defragmentace znamená, že namísto pokusu o využití hello toodistribute metriky napříč hello clusteru, je jsou konsolidovány.</span><span class="sxs-lookup"><span data-stu-id="98778-108">Defragmentation means that instead of trying toodistribute hello utilization of a metric across hello cluster, it is consolidated.</span></span> <span data-ttu-id="98778-109">Konsolidace je právě inverzi hello výchozí vyrovnávání strategie – místo minimalizovat hello průměrná směrodatnou odchylku metriky zatížení, hello správce prostředků clusteru pokusí tooincrease ho.</span><span class="sxs-lookup"><span data-stu-id="98778-109">Consolidation is just an inversion of hello default balancing strategy – instead of minimizing hello average standard deviation of metric load, hello Cluster Resource Manager tries tooincrease it.</span></span>

## <a name="when-toouse-defragmentation"></a><span data-ttu-id="98778-110">Když toouse Defragmentace</span><span class="sxs-lookup"><span data-stu-id="98778-110">When toouse defragmentation</span></span>
<span data-ttu-id="98778-111">Distribuce zatížení v clusteru hello spotřebuje některé hello prostředků na každém uzlu.</span><span class="sxs-lookup"><span data-stu-id="98778-111">Distributing load in hello cluster consumes some of hello resources on each node.</span></span> <span data-ttu-id="98778-112">Některé úlohy vytvoření služby, které jsou velmi velké a využívat většinu nebo všechny uzlu.</span><span class="sxs-lookup"><span data-stu-id="98778-112">Some workloads create services that are exceptionally large and consume most or all of a node.</span></span> <span data-ttu-id="98778-113">V těchto případech je možné, že pokud jsou velké získávání vytvoření úlohy, které není k dispozici dostatek místo na všechny toorun uzlu je.</span><span class="sxs-lookup"><span data-stu-id="98778-113">In these cases, it's possible that when there are large workloads getting created that there isn't enough space on any node toorun them.</span></span> <span data-ttu-id="98778-114">Rozsáhlejší úlohy nejsou potíže s Service Fabric; v těchto případech hello clusteru Resource Manager určí, že potřebuje tooreorganize hello clusteru toomake místo pro tento velké zatížení.</span><span class="sxs-lookup"><span data-stu-id="98778-114">Large workloads aren't a problem in Service Fabric; in these cases hello Cluster Resource Manager determines that it needs tooreorganize hello cluster toomake room for this large workload.</span></span> <span data-ttu-id="98778-115">Ale v hello zatím této úlohy má toowait toobe naplánované v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="98778-115">However, in hello meantime that workload has toowait toobe scheduled in hello cluster.</span></span>

<span data-ttu-id="98778-116">Pokud máte mnoho služeb a stavu toomove kolem, může to trvat dlouho toobe velké zatížení hello umístit do clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="98778-116">If there are many services and state toomove around, then it could take a long time for hello large workload toobe placed in hello cluster.</span></span> <span data-ttu-id="98778-117">Toto je pravděpodobnější, pokud dalších zatížení v clusteru hello také velké a proto trvat déle tooreorganize.</span><span class="sxs-lookup"><span data-stu-id="98778-117">This is more likely if other workloads in hello cluster are also large and so take longer tooreorganize.</span></span> <span data-ttu-id="98778-118">tým Service Fabric Hello měří času vytvoření v simulace tohoto scénáře.</span><span class="sxs-lookup"><span data-stu-id="98778-118">hello Service Fabric team measured creation times in simulations of this scenario.</span></span> <span data-ttu-id="98778-119">Zjistili jsme, že trvalo déle, mnohem vytváření velké služeb, při využití clusteru získali výše 30 až 50 %.</span><span class="sxs-lookup"><span data-stu-id="98778-119">We found that creating large services took much longer as soon as cluster utilization got above between 30% and 50%.</span></span> <span data-ttu-id="98778-120">toohandle tento scénář zavedli jsme defragmentace jako strategie vyrovnávání.</span><span class="sxs-lookup"><span data-stu-id="98778-120">toohandle this scenario, we introduced defragmentation as a balancing strategy.</span></span> <span data-ttu-id="98778-121">Zjistili jsme, že pro větší zatížení, zejména ty, kde je důležité, defragmentace skutečně pomohly tyto nové úlohy vytváření získat naplánován v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="98778-121">We found that for large workloads, especially ones where creation time was important, defragmentation really helped those new workloads get scheduled in hello cluster.</span></span>

<span data-ttu-id="98778-122">Defragmentace metriky toohave hello správce prostředků clusteru tooproactively zkuste toocondense hello zatížení hello služeb můžete nakonfigurovat do menšího počtu uzlů.</span><span class="sxs-lookup"><span data-stu-id="98778-122">You can configure defragmentation metrics toohave hello Cluster Resource Manager tooproactively try toocondense hello load of hello services into fewer nodes.</span></span> <span data-ttu-id="98778-123">To pomáhá zajistit, že je téměř vždy prostor pro velké služeb bez reorganizace hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="98778-123">This helps ensure that there is almost always room for large services without reorganizing hello cluster.</span></span> <span data-ttu-id="98778-124">Nemá tooreorganize hello clusteru umožňuje vytváření rozsáhlých úloh s rychle.</span><span class="sxs-lookup"><span data-stu-id="98778-124">Not having tooreorganize hello cluster allows creating large workloads quickly.</span></span>

<span data-ttu-id="98778-125">Většina lidí nepotřebují defragmentace.</span><span class="sxs-lookup"><span data-stu-id="98778-125">Most people don’t need defragmentation.</span></span> <span data-ttu-id="98778-126">Služby jsou obvykle být malé, takže není pevný toofind prostor pro je v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="98778-126">Services are usually be small, so it’s not hard toofind room for them in hello cluster.</span></span> <span data-ttu-id="98778-127">Když reorganizaci je možné, přejde rychle znovu protože většina služeb jsou malé a přesunout rychle a paralelně.</span><span class="sxs-lookup"><span data-stu-id="98778-127">When reorganization is possible, it goes quickly, again because most services are small and can be moved quickly and in parallel.</span></span> <span data-ttu-id="98778-128">Ale pokud máte velké služby a potřeby rychle vytvořit můžete hello defragmentace strategie je pro vás.</span><span class="sxs-lookup"><span data-stu-id="98778-128">However, if you have large services and need them created quickly then hello defragmentation strategy is for you.</span></span> <span data-ttu-id="98778-129">Probereme hello kompromisy defragmentace další použití.</span><span class="sxs-lookup"><span data-stu-id="98778-129">We'll discuss hello tradeoffs of using defragmentation next.</span></span> 

## <a name="defragmentation-tradeoffs"></a><span data-ttu-id="98778-130">Defragmentace kompromisy</span><span class="sxs-lookup"><span data-stu-id="98778-130">Defragmentation tradeoffs</span></span>
<span data-ttu-id="98778-131">Defragmentace může zvýšit impactfulness chyb, protože běží další služby na uzlech, které nesplní.</span><span class="sxs-lookup"><span data-stu-id="98778-131">Defragmentation can increase impactfulness of failures, since more services are running on nodes that fail.</span></span> <span data-ttu-id="98778-132">Defragmentace můžete taky zvýšit náklady, protože prostředků v clusteru hello musí uchovávat v rezervy, čekání na vytvoření hello z rozsáhlejší úlohy.</span><span class="sxs-lookup"><span data-stu-id="98778-132">Defragmentation can also increase costs, since resources in hello cluster must be held in reserve, waiting for hello creation of large workloads.</span></span>

<span data-ttu-id="98778-133">Hello následující obrázek poskytuje vizuální reprezentace dva clustery, ten, který je defragmentovat a ten, který není.</span><span class="sxs-lookup"><span data-stu-id="98778-133">hello following diagram gives a visual representation of two clusters, one that is defragmented and one that is not.</span></span> 

<span data-ttu-id="98778-134"><center>
![Porovnání vyrovnáváním a defragmentovat clustery][Image1]
</center></span><span class="sxs-lookup"><span data-stu-id="98778-134"><center>
![Comparing Balanced and Defragmented Clusters][Image1]
</center></span></span>

<span data-ttu-id="98778-135">V případě hello vyrovnáváním vezměte v úvahu počet hello pohybů, které by bylo nutné tooplace mezi největší objekty služby hello.</span><span class="sxs-lookup"><span data-stu-id="98778-135">In hello balanced case, consider hello number of movements that would be necessary tooplace one of hello largest service objects.</span></span> <span data-ttu-id="98778-136">V clusteru defragmentované hello může být umístěny na uzlů čtyř nebo pěti hello velkých úloh bez nutnosti toowait pro jiné služby toomove.</span><span class="sxs-lookup"><span data-stu-id="98778-136">In hello defragmented cluster, hello large workload could be placed on nodes four or five without having toowait for any other services toomove.</span></span>

## <a name="defragmentation-pros-and-cons"></a><span data-ttu-id="98778-137">Defragmentace výhody a nevýhody</span><span class="sxs-lookup"><span data-stu-id="98778-137">Defragmentation pros and cons</span></span>
<span data-ttu-id="98778-138">Jaké jsou proto tyto koncepční kompromisy?</span><span class="sxs-lookup"><span data-stu-id="98778-138">So what are those other conceptual tradeoffs?</span></span> <span data-ttu-id="98778-139">Tady je rychlý tabulku věcí toothink o:</span><span class="sxs-lookup"><span data-stu-id="98778-139">Here’s a quick table of things toothink about:</span></span>

| <span data-ttu-id="98778-140">Defragmentace specialisté</span><span class="sxs-lookup"><span data-stu-id="98778-140">Defragmentation Pros</span></span> | <span data-ttu-id="98778-141">Cons Defragmentace</span><span class="sxs-lookup"><span data-stu-id="98778-141">Defragmentation Cons</span></span> |
| --- | --- |
| <span data-ttu-id="98778-142">Umožňuje rychlejší vytváření velké služeb</span><span class="sxs-lookup"><span data-stu-id="98778-142">Allows faster creation of large services</span></span> |<span data-ttu-id="98778-143">Koncentráty načíst do menšího počtu uzlů, zvýšení kolizí</span><span class="sxs-lookup"><span data-stu-id="98778-143">Concentrates load onto fewer nodes, increasing contention</span></span> |
| <span data-ttu-id="98778-144">Umožňuje snížit přesun dat během vytváření</span><span class="sxs-lookup"><span data-stu-id="98778-144">Enables lower data movement during creation</span></span> |<span data-ttu-id="98778-145">Selhání může mít vliv na další služby a způsobit další změny</span><span class="sxs-lookup"><span data-stu-id="98778-145">Failures can impact more services and cause more churn</span></span> |
| <span data-ttu-id="98778-146">Umožňuje bohaté popis požadavků a recyklace místa</span><span class="sxs-lookup"><span data-stu-id="98778-146">Allows rich description of requirements and reclamation of space</span></span> |<span data-ttu-id="98778-147">Složitější celkové konfiguraci správy prostředků</span><span class="sxs-lookup"><span data-stu-id="98778-147">More complex overall Resource Management configuration</span></span> |

<span data-ttu-id="98778-148">Je možné kombinovat defragmentované a normální metriky v hello stejného clusteru.</span><span class="sxs-lookup"><span data-stu-id="98778-148">You can mix defragmented and normal metrics in hello same cluster.</span></span> <span data-ttu-id="98778-149">Hello správce prostředků clusteru pokusů tooconsolidate hello defragmentace metriky co nejvíce při rozšíří hello ostatní.</span><span class="sxs-lookup"><span data-stu-id="98778-149">hello Cluster Resource Manager tries tooconsolidate hello defragmentation metrics as much as possible while spreading out hello others.</span></span> <span data-ttu-id="98778-150">výsledky Hello kombinování defragmentace a vyrovnávání strategie závisí na několika různými faktory, včetně:</span><span class="sxs-lookup"><span data-stu-id="98778-150">hello results of mixing defragmentation and balancing strategies depends on several factors, including:</span></span>
  - <span data-ttu-id="98778-151">počet Hello vyrovnávání metriky oproti hello počet defragmentace metriky</span><span class="sxs-lookup"><span data-stu-id="98778-151">hello number of balancing metrics vs. hello number of defragmentation metrics</span></span>
  - <span data-ttu-id="98778-152">Tom, zda všechny služby používá oba typy metriky</span><span class="sxs-lookup"><span data-stu-id="98778-152">Whether any service uses both types of metrics</span></span> 
  - <span data-ttu-id="98778-153">metriky váhu Hello</span><span class="sxs-lookup"><span data-stu-id="98778-153">hello metric weights</span></span>
  - <span data-ttu-id="98778-154">načte aktuální metrika</span><span class="sxs-lookup"><span data-stu-id="98778-154">current metric loads</span></span>
  
<span data-ttu-id="98778-155">Experimentování je požadovaná toodetermine hello přesnou konfiguraci nezbytné.</span><span class="sxs-lookup"><span data-stu-id="98778-155">Experimentation is required toodetermine hello exact configuration necessary.</span></span> <span data-ttu-id="98778-156">Před povolením defragmentace metriky v produkčním prostředí doporučujeme důkladné měření vašich úloh.</span><span class="sxs-lookup"><span data-stu-id="98778-156">We recommend thorough measurement of your workloads before you enable defragmentation metrics in production.</span></span> <span data-ttu-id="98778-157">To je užitečné především při kombinování defragmentace a vyrovnáváním metriky v rámci hello stejnou službu.</span><span class="sxs-lookup"><span data-stu-id="98778-157">This is especially true when mixing defragmentation and balanced metrics within hello same service.</span></span> 

## <a name="configuring-defragmentation-metrics"></a><span data-ttu-id="98778-158">Konfigurace defragmentace metriky</span><span class="sxs-lookup"><span data-stu-id="98778-158">Configuring defragmentation metrics</span></span>
<span data-ttu-id="98778-159">Konfigurace metriky defragmentace je globální rozhodnutí v clusteru hello a jednotlivé metriky můžete vybrat pro defragmentaci.</span><span class="sxs-lookup"><span data-stu-id="98778-159">Configuring defragmentation metrics is a global decision in hello cluster, and individual metrics can be selected for defragmentation.</span></span> <span data-ttu-id="98778-160">Hello následující konfigurace fragmenty kódu ukazují, jak tooconfigure metriky pro defragmentace.</span><span class="sxs-lookup"><span data-stu-id="98778-160">hello following config snippets show how tooconfigure metrics for defragmentation.</span></span> <span data-ttu-id="98778-161">V tomto případě "Metric1" je nakonfigurovaná jako defragmentace metriku, zatímco "Metric2" bude dále toobe vyrovnáváním normálně.</span><span class="sxs-lookup"><span data-stu-id="98778-161">In this case, "Metric1" is configured as a defragmentation metric, while "Metric2" will continue toobe balanced normally.</span></span> 

<span data-ttu-id="98778-162">ClusterManifest.xml:</span><span class="sxs-lookup"><span data-stu-id="98778-162">ClusterManifest.xml:</span></span>

```xml
<Section Name="DefragmentationMetrics">
    <Parameter Name="Metric1" Value="true" />
    <Parameter Name="Metric2" Value="false" />
</Section>
```

<span data-ttu-id="98778-163">pomocí souboru ClusterConfig.json pro samostatné nasazení nebo Template.json pro Azure hostované clustery:</span><span class="sxs-lookup"><span data-stu-id="98778-163">via ClusterConfig.json for Standalone deployments or Template.json for Azure hosted clusters:</span></span>

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


## <a name="next-steps"></a><span data-ttu-id="98778-164">Další kroky</span><span class="sxs-lookup"><span data-stu-id="98778-164">Next steps</span></span>
- <span data-ttu-id="98778-165">Hello správce prostředků clusteru má man možnosti popisující hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="98778-165">hello Cluster Resource Manager has man options for describing hello cluster.</span></span> <span data-ttu-id="98778-166">toofind Další informace o jejich, projděte si tento článek na [popisující cluster Service Fabric](service-fabric-cluster-resource-manager-cluster-description.md)</span><span class="sxs-lookup"><span data-stu-id="98778-166">toofind out more about them, check out this article on [describing a Service Fabric cluster](service-fabric-cluster-resource-manager-cluster-description.md)</span></span>
- <span data-ttu-id="98778-167">Metriky se, jak hello správce prostředků clusteru Service Fabric spravuje využívání a kapacity v clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="98778-167">Metrics are how hello Service Fabric Cluster Resource Manger manages consumption and capacity in hello cluster.</span></span> <span data-ttu-id="98778-168">Další informace o metrikách toolearn a jak tooconfigure, podívejte se na [v tomto článku](service-fabric-cluster-resource-manager-metrics.md)</span><span class="sxs-lookup"><span data-stu-id="98778-168">toolearn more about metrics and how tooconfigure them, check out [this article](service-fabric-cluster-resource-manager-metrics.md)</span></span>

[Image1]:./media/service-fabric-cluster-resource-manager-defragmentation-metrics/balancing-defrag-compared.png
