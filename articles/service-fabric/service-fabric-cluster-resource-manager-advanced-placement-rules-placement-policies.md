---
title: "Správce prostředků clusteru Service Fabric - zásady umístění | Microsoft Docs"
description: "Přehled další umístění zásad a pravidel pro služby Service Fabric"
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: 5c2d19c6-dd40-4c4b-abd3-5c5ec0abed38
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 6c11d49d5fdb3148b0534c9448f815358fa8cab3
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="placement-policies-for-service-fabric-services"></a><span data-ttu-id="87129-103">Umístění zásady pro služby service fabric</span><span class="sxs-lookup"><span data-stu-id="87129-103">Placement policies for service fabric services</span></span>
<span data-ttu-id="87129-104">Umístění zásady jsou další pravidla, které lze použít k řízení umístění služby v některých scénářích konkrétní, méně běžné.</span><span class="sxs-lookup"><span data-stu-id="87129-104">Placement policies are additional rules that can be used to govern service placement in some specific, less-common scenarios.</span></span> <span data-ttu-id="87129-105">Mezi příklady tyto scénáře patří:</span><span class="sxs-lookup"><span data-stu-id="87129-105">Some examples of those scenarios are:</span></span>

- <span data-ttu-id="87129-106">Cluster Service Fabric zahrnuje geografické vzdálenosti, například několik datových center v místě nebo mezi oblastmi Azure</span><span class="sxs-lookup"><span data-stu-id="87129-106">Your Service Fabric cluster spans geographic distances, such as multiple on-premises datacenters or across Azure regions</span></span>
- <span data-ttu-id="87129-107">Vaše prostředí zahrnuje více oblastí řízení geopolitické nebo právních nebo některých jiných případu, kdy máte zásady potřebujete vynutit hranice</span><span class="sxs-lookup"><span data-stu-id="87129-107">Your environment spans multiple areas of geopolitical or legal control, or some other case where you have policy boundaries you need to enforce</span></span>
- <span data-ttu-id="87129-108">Výkon nebo latenci rozhodnutí z důvodu dlouhé vzdálenosti nebo použití odkazů pomalejší nebo méně spolehlivé síťové komunikace</span><span class="sxs-lookup"><span data-stu-id="87129-108">There are communication performance or latency considerations due to large distances or use of slower or less reliable network links</span></span>
- <span data-ttu-id="87129-109">Je třeba zachovat určité úlohy společně umístěná jako nejlepší úsilí, s ostatními úlohami, nebo v blízkosti zákazníkům</span><span class="sxs-lookup"><span data-stu-id="87129-109">You need to keep certain workloads collocated as a best effort, either with other workloads or in proximity to customers</span></span>

<span data-ttu-id="87129-110">Většinu těchto požadavků zarovnané s fyzické rozložení clusteru, vyjádřené domén selhání clusteru.</span><span class="sxs-lookup"><span data-stu-id="87129-110">Most of these requirements align with the physical layout of the cluster, represented as the fault domains of the cluster.</span></span> 

<span data-ttu-id="87129-111">Zásady rozšířené umístění, které pomůžou adresy jsou tyto scénáře:</span><span class="sxs-lookup"><span data-stu-id="87129-111">The advanced placement policies that help address these scenarios are:</span></span>

1. <span data-ttu-id="87129-112">Neplatný domén</span><span class="sxs-lookup"><span data-stu-id="87129-112">Invalid domains</span></span>
2. <span data-ttu-id="87129-113">Požadované domén</span><span class="sxs-lookup"><span data-stu-id="87129-113">Required domains</span></span>
3. <span data-ttu-id="87129-114">Upřednostňovaných domén</span><span class="sxs-lookup"><span data-stu-id="87129-114">Preferred domains</span></span>
4. <span data-ttu-id="87129-115">Zakazuje se okolních repliky</span><span class="sxs-lookup"><span data-stu-id="87129-115">Disallowing replica packing</span></span>

<span data-ttu-id="87129-116">Většina následující ovládacích prvků se daly konfigurovat prostřednictvím vlastnosti uzlu a omezení umístění, ale některé jsou složitější.</span><span class="sxs-lookup"><span data-stu-id="87129-116">Most of the following controls could be configured via node properties and placement constraints, but some are more complicated.</span></span> <span data-ttu-id="87129-117">Chcete-li věcí jednodušší, správce prostředků clusteru Service Fabric poskytuje tyto zásady další umístění.</span><span class="sxs-lookup"><span data-stu-id="87129-117">To make things simpler, the Service Fabric Cluster Resource Manager provides these additional placement policies.</span></span> <span data-ttu-id="87129-118">Konfigurace zásad umístění na základě služby za názvem instance.</span><span class="sxs-lookup"><span data-stu-id="87129-118">Placement policies are configured on a per-named service instance basis.</span></span> <span data-ttu-id="87129-119">Se dá se taky aktualizovat dynamicky.</span><span class="sxs-lookup"><span data-stu-id="87129-119">They can also be updated dynamically.</span></span>

## <a name="specifying-invalid-domains"></a><span data-ttu-id="87129-120">Neplatné zadání domén</span><span class="sxs-lookup"><span data-stu-id="87129-120">Specifying invalid domains</span></span>
<span data-ttu-id="87129-121">**InvalidDomain** umístění zásady umožňuje určit, že je neplatný pro konkrétní službu konkrétní domény selhání.</span><span class="sxs-lookup"><span data-stu-id="87129-121">The **InvalidDomain** placement policy allows you to specify that a particular Fault Domain is invalid for a specific service.</span></span> <span data-ttu-id="87129-122">Tato zásada zajišťuje, že konkrétní službu nikdy běží v určité oblasti, například důvodů geopolitické nebo podnikové zásady.</span><span class="sxs-lookup"><span data-stu-id="87129-122">This policy ensures that a particular service never runs in a particular area, for example for geopolitical or corporate policy reasons.</span></span> <span data-ttu-id="87129-123">Přes samostatné zásady je možné zadat více domén neplatný.</span><span class="sxs-lookup"><span data-stu-id="87129-123">Multiple invalid domains may be specified via separate policies.</span></span>

<span data-ttu-id="87129-124"><center>
![Příklad domény je neplatný][Image1]
</center></span><span class="sxs-lookup"><span data-stu-id="87129-124"><center>
![Invalid Domain Example][Image1]
</center></span></span>

<span data-ttu-id="87129-125">Kód:</span><span class="sxs-lookup"><span data-stu-id="87129-125">Code:</span></span>

```csharp
ServicePlacementInvalidDomainPolicyDescription invalidDomain = new ServicePlacementInvalidDomainPolicyDescription();
invalidDomain.DomainName = "fd:/DCEast"; //regulations prohibit this workload here
serviceDescription.PlacementPolicies.Add(invalidDomain);
```

<span data-ttu-id="87129-126">Prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="87129-126">Powershell:</span></span>

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("InvalidDomain,fd:/DCEast”)
```
## <a name="specifying-required-domains"></a><span data-ttu-id="87129-127">Určení požadované domén</span><span class="sxs-lookup"><span data-stu-id="87129-127">Specifying required domains</span></span>
<span data-ttu-id="87129-128">Zásady požadovanou doménu umístění vyžaduje, aby služba pouze v zadané doméně nenachází.</span><span class="sxs-lookup"><span data-stu-id="87129-128">The required domain placement policy requires that the service is present only in the specified domain.</span></span> <span data-ttu-id="87129-129">Lze zadat více domén požadované přes samostatné zásady.</span><span class="sxs-lookup"><span data-stu-id="87129-129">Multiple required domains can be specified via separate policies.</span></span>

<span data-ttu-id="87129-130"><center>
![Příklad požadovanou doménu][Image2]
</center></span><span class="sxs-lookup"><span data-stu-id="87129-130"><center>
![Required Domain Example][Image2]
</center></span></span>

<span data-ttu-id="87129-131">Kód:</span><span class="sxs-lookup"><span data-stu-id="87129-131">Code:</span></span>

```csharp
ServicePlacementRequiredDomainPolicyDescription requiredDomain = new ServicePlacementRequiredDomainPolicyDescription();
requiredDomain.DomainName = "fd:/DC01/RK03/BL2";
serviceDescription.PlacementPolicies.Add(requiredDomain);
```

<span data-ttu-id="87129-132">Prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="87129-132">Powershell:</span></span>

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("RequiredDomain,fd:/DC01/RK03/BL2")
```

## <a name="specifying-a-preferred-domain-for-the-primary-replicas-of-a-stateful-service"></a><span data-ttu-id="87129-133">Určení upřednostňované domény pro primární repliky stavové služby</span><span class="sxs-lookup"><span data-stu-id="87129-133">Specifying a preferred domain for the primary replicas of a stateful service</span></span>
<span data-ttu-id="87129-134">Upřednostňovaný primární doménu Určuje domény selhání umístit primární v.</span><span class="sxs-lookup"><span data-stu-id="87129-134">The Preferred Primary Domain specifies the fault domain to place the Primary in.</span></span> <span data-ttu-id="87129-135">Primární skončilo v této doméně při všechno, co je v pořádku.</span><span class="sxs-lookup"><span data-stu-id="87129-135">The Primary ends up in this domain when everything is healthy.</span></span> <span data-ttu-id="87129-136">Pokud doméně nebo primární repliky selže nebo vypne, primární přesune do jiného umístění, nejlépe ve stejné doméně.</span><span class="sxs-lookup"><span data-stu-id="87129-136">If the domain or the Primary replica fails or shuts down, the Primary moves to some other location, ideally in the same domain.</span></span> <span data-ttu-id="87129-137">Není-li toto nové umístění v upřednostňované doméně, správce prostředků clusteru přesouvá ji zpět do upřednostňovaných domény co nejdříve.</span><span class="sxs-lookup"><span data-stu-id="87129-137">If this new location isn't in the preferred domain, the Cluster Resource Manager moves it back to the preferred domain as soon as possible.</span></span> <span data-ttu-id="87129-138">Přirozeně toto nastavení má smysl jenom pro stavové služby.</span><span class="sxs-lookup"><span data-stu-id="87129-138">Naturally this setting only makes sense for stateful services.</span></span> <span data-ttu-id="87129-139">Tato zásada je velmi užitečné v clusterech, které jsou rozloženy oblastí Azure nebo několik datových center ale máte služby, které dávají přednost umístění v určitých umístění.</span><span class="sxs-lookup"><span data-stu-id="87129-139">This policy is most useful in clusters that are spanned across Azure regions or multiple datacenters but have services that prefer placement in a certain location.</span></span> <span data-ttu-id="87129-140">Zachovat základní barvy blízko uživatelů nebo jiné služby pomáhá poskytovat nižší latenci, hlavně pro čtení, které jsou zpracovávány základní barvy ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="87129-140">Keeping Primaries close to their users or other services helps provide lower latency, especially for reads, which are handled by Primaries by default.</span></span>

<span data-ttu-id="87129-141"><center>
![Upřednostňovaný primární domény a převzetí služeb při selhání][Image3]
</center></span><span class="sxs-lookup"><span data-stu-id="87129-141"><center>
![Preferred Primary Domains and Failover][Image3]
</center></span></span>

```csharp
ServicePlacementPreferPrimaryDomainPolicyDescription primaryDomain = new ServicePlacementPreferPrimaryDomainPolicyDescription();
primaryDomain.DomainName = "fd:/EastUS/";
serviceDescription.PlacementPolicies.Add(invalidDomain);
```

<span data-ttu-id="87129-142">Prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="87129-142">Powershell:</span></span>

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("PreferredPrimaryDomain,fd:/EastUS")
```

## <a name="requiring-replica-distribution-and-disallowing-packing"></a><span data-ttu-id="87129-143">Vyžadování distribučních repliky a zakazuje okolních</span><span class="sxs-lookup"><span data-stu-id="87129-143">Requiring replica distribution and disallowing packing</span></span>
<span data-ttu-id="87129-144">Repliky jsou _normálně_ distribuované napříč doménami selhání a upgradu, až bude cluster v pořádku.</span><span class="sxs-lookup"><span data-stu-id="87129-144">Replicas are _normally_ distributed across fault and upgrade domains when the cluster is healthy.</span></span> <span data-ttu-id="87129-145">Existují však případy, kdy může se stát víc než jednu repliku pro daný oddíl dočasně sbalené do jedné domény.</span><span class="sxs-lookup"><span data-stu-id="87129-145">However, there are cases where more than one replica for a given partition may end up temporarily packed into a single domain.</span></span> <span data-ttu-id="87129-146">Předpokládejme například, že aby cluster má devět uzly v tři domén selhání, fd: / 0, fd: / 1 a fd: / 2.</span><span class="sxs-lookup"><span data-stu-id="87129-146">For example, let's say that the cluster has nine nodes in three fault domains, fd:/0, fd:/1, and fd:/2.</span></span> <span data-ttu-id="87129-147">Dále předpokládejme, že vaše služba má tři repliky.</span><span class="sxs-lookup"><span data-stu-id="87129-147">Let's also say that your service has three replicas.</span></span> <span data-ttu-id="87129-148">Řekněme, že uzly, které byly používány pro tyto repliky v fd: / 1 a fd: / 2 byl vypnut.</span><span class="sxs-lookup"><span data-stu-id="87129-148">Let's say that the nodes that were being used for those replicas in fd:/1 and fd:/2 went down.</span></span> <span data-ttu-id="87129-149">Obvykle správce prostředků clusteru přejete jiné uzly v těchto stejné domény selhání.</span><span class="sxs-lookup"><span data-stu-id="87129-149">Normally the Cluster Resource Manager would prefer other nodes in those same fault domains.</span></span> <span data-ttu-id="87129-150">V takovém případě Řekněme kvůli problémům s kapacitu, že žádný z jiných uzlů v těchto doménách nebyly platné.</span><span class="sxs-lookup"><span data-stu-id="87129-150">In this case, let's say due to capacity issues none of the other nodes in those domains were valid.</span></span> <span data-ttu-id="87129-151">Pokud správce prostředků clusteru sestavení nahrazení pro tyto repliky, by mělo zvolte uzlů v fd: / 0.</span><span class="sxs-lookup"><span data-stu-id="87129-151">If the Cluster Resource Manager builds replacements for those replicas, it would have to choose nodes in fd:/0.</span></span> <span data-ttu-id="87129-152">To však _,_ vytvoří situaci, kdy porušení omezení domény selhání.</span><span class="sxs-lookup"><span data-stu-id="87129-152">However, doing _that_ creates a situation where the Fault Domain constraint is violated.</span></span> <span data-ttu-id="87129-153">Balení zvyšuje repliky může pravděpodobnost, že nastavení celou repliku přejděte nebo ztratí.</span><span class="sxs-lookup"><span data-stu-id="87129-153">Packing replicas increases the chance that the whole replica set could go down or be lost.</span></span> 

> [!NOTE]
> <span data-ttu-id="87129-154">Další informace o omezení a omezení priority obecně platí, podívejte se na [v tomto tématu](service-fabric-cluster-resource-manager-management-integration.md#constraint-priorities).</span><span class="sxs-lookup"><span data-stu-id="87129-154">For more information on constraints and constraint priorities generally, check out [this topic](service-fabric-cluster-resource-manager-management-integration.md#constraint-priorities).</span></span>
>

<span data-ttu-id="87129-155">Pokud někdy vidíte zprávu stavu, jako "`The Load Balancer has detected a Constraint Violation for this Replica:fabric:/<some service name> Secondary Partition <some partition ID> is violating the Constraint: FaultDomain`", pak jste stisknutí tlačítka tato podmínka nebo něco podobného jako ho.</span><span class="sxs-lookup"><span data-stu-id="87129-155">If you've ever seen a health message such as "`The Load Balancer has detected a Constraint Violation for this Replica:fabric:/<some service name> Secondary Partition <some partition ID> is violating the Constraint: FaultDomain`", then you've hit this condition or something like it.</span></span> <span data-ttu-id="87129-156">Obvykle pouze jednu nebo dvě repliky jsou společně zabaleny dočasně.</span><span class="sxs-lookup"><span data-stu-id="87129-156">Usually only one or two replicas are packed together temporarily.</span></span> <span data-ttu-id="87129-157">Tak dlouho, dokud je méně než kvorum repliky v dané doméně, jste bezpečné.</span><span class="sxs-lookup"><span data-stu-id="87129-157">So long as there are fewer than a quorum of replicas in a given domain, you're safe.</span></span> <span data-ttu-id="87129-158">Okolních je taková situace vzácná, ale může se stát, a obvykle jsou tyto situace přechodný vzhledem k tomu, že uzly se vraťte.</span><span class="sxs-lookup"><span data-stu-id="87129-158">Packing is rare, but it can happen, and usually these situations are transient since the nodes come back.</span></span> <span data-ttu-id="87129-159">Pokud uzly zůstat dolů a správce prostředků clusteru je potřeba vytvořit náhrady, obvykle existuje jiné uzly v doménách selhání ideální.</span><span class="sxs-lookup"><span data-stu-id="87129-159">If the nodes do stay down and the Cluster Resource Manager needs to build replacements, usually there are other nodes available in the ideal fault domains.</span></span>

<span data-ttu-id="87129-160">Některé úlohy by raději vždy s cílovým počtem replik, i když jsou zabaleny do menšího počtu domén.</span><span class="sxs-lookup"><span data-stu-id="87129-160">Some workloads would prefer always having the target number of replicas, even if they are packed into fewer domains.</span></span> <span data-ttu-id="87129-161">Tyto úlohy jsou ale proti selhání celkový počet souběžných trvalé domény a obvykle můžete obnovit místní stavu.</span><span class="sxs-lookup"><span data-stu-id="87129-161">These workloads are betting against total simultaneous permanent domain failures and can usually recover local state.</span></span> <span data-ttu-id="87129-162">Další úlohy by starší než riziko správnost nebo ke ztrátě dat. místo trvat výpadek.</span><span class="sxs-lookup"><span data-stu-id="87129-162">Other workloads would rather take the downtime earlier than risk correctness or loss of data.</span></span> <span data-ttu-id="87129-163">Většina produkční úlohy spustit s víc než tři repliky, víc než třemi domén selhání a mnoho platný uzlů na domény selhání.</span><span class="sxs-lookup"><span data-stu-id="87129-163">Most production workloads run with more than three replicas, more than three fault domains, and many valid nodes per fault domain.</span></span> <span data-ttu-id="87129-164">Z tohoto důvodu se výchozí chování umožňuje okolních domény ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="87129-164">Because of this, the default behavior allows domain packing by default.</span></span> <span data-ttu-id="87129-165">Výchozí chování umožňuje normální vyrovnávání a převzetí služeb při selhání pro zpracování těchto výjimečných případech i v případě, že to znamená okolních dočasné domény.</span><span class="sxs-lookup"><span data-stu-id="87129-165">The default behavior allows normal balancing and failover to handle these extreme cases, even if that means temporary domain packing.</span></span>

<span data-ttu-id="87129-166">Pokud chcete zakázat tyto obaly pro danou úlohu, můžete zadat `RequireDomainDistribution` zásady ve službě.</span><span class="sxs-lookup"><span data-stu-id="87129-166">If you want to disable such packing for a given workload, you can specify the `RequireDomainDistribution` policy on the service.</span></span> <span data-ttu-id="87129-167">Pokud je tato zásada nastavená, zajišťuje správce prostředků clusteru spustí žádné dvě repliky ze stejného oddílu ve stejné doméně selhání nebo upgradu.</span><span class="sxs-lookup"><span data-stu-id="87129-167">When this policy is set, the Cluster Resource Manager ensures no two replicas from the same partition run in the same fault or upgrade domain.</span></span>

<span data-ttu-id="87129-168">Kód:</span><span class="sxs-lookup"><span data-stu-id="87129-168">Code:</span></span>

```csharp
ServicePlacementRequireDomainDistributionPolicyDescription distributeDomain = new ServicePlacementRequireDomainDistributionPolicyDescription();
serviceDescription.PlacementPolicies.Add(distributeDomain);
```

<span data-ttu-id="87129-169">Prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="87129-169">Powershell:</span></span>

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("RequiredDomainDistribution")
```

<span data-ttu-id="87129-170">Teď by bylo možné použít tyto konfigurace pro služby v clusteru, který nebyl předané geograficky?</span><span class="sxs-lookup"><span data-stu-id="87129-170">Now, would it be possible to use these configurations for services in a cluster that was not geographically spanned?</span></span> <span data-ttu-id="87129-171">Vám může, ale není skvělé důvod příliš.</span><span class="sxs-lookup"><span data-stu-id="87129-171">You could, but there’s not a great reason too.</span></span> <span data-ttu-id="87129-172">Pokud je vyžadují scénáře je nutno konfigurace vyžaduje, neplatný a upřednostňované domény.</span><span class="sxs-lookup"><span data-stu-id="87129-172">The required, invalid, and preferred domain configurations should be avoided unless the scenarios require them.</span></span> <span data-ttu-id="87129-173">Nemá smysl žádné pokusí vynutit daného zatížení spustit v jednom stojanu nebo raději některé segment místním clusteru oproti jinému.</span><span class="sxs-lookup"><span data-stu-id="87129-173">It doesn't make any sense to try to force a given workload to run in a single rack, or to prefer some segment of your local cluster over another.</span></span> <span data-ttu-id="87129-174">Různé hardwarové konfigurace by měl být rozloženy domén selhání a zpracovávají pomocí normální umístění omezení a vlastnosti uzlu.</span><span class="sxs-lookup"><span data-stu-id="87129-174">Different hardware configurations should be spread across fault domains and handled via normal placement constraints and node properties.</span></span>

## <a name="next-steps"></a><span data-ttu-id="87129-175">Další kroky</span><span class="sxs-lookup"><span data-stu-id="87129-175">Next steps</span></span>
- <span data-ttu-id="87129-176">Další informace o konfiguraci služby [Další informace o konfiguraci služby](service-fabric-cluster-resource-manager-configure-services.md)</span><span class="sxs-lookup"><span data-stu-id="87129-176">For more information on configuring services, [Learn about configuring Services](service-fabric-cluster-resource-manager-configure-services.md)</span></span>

[Image1]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies/cluster-invalid-placement-domain.png
[Image2]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies/cluster-required-placement-domain.png
[Image3]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies/cluster-preferred-primary-domain.png
