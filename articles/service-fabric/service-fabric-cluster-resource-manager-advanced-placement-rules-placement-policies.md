---
title: "aaaService správce prostředků clusteru prostředků infrastruktury – zásady umístění | Microsoft Docs"
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
ms.openlocfilehash: bb58642520085ab3000f3929cf9aea7a8f6e3070
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="placement-policies-for-service-fabric-services"></a><span data-ttu-id="086fc-103">Umístění zásady pro služby service fabric</span><span class="sxs-lookup"><span data-stu-id="086fc-103">Placement policies for service fabric services</span></span>
<span data-ttu-id="086fc-104">Zásady umístění se další pravidla, která může být umístění služby použít toogovern v některých scénářích konkrétní, méně běžné.</span><span class="sxs-lookup"><span data-stu-id="086fc-104">Placement policies are additional rules that can be used toogovern service placement in some specific, less-common scenarios.</span></span> <span data-ttu-id="086fc-105">Mezi příklady tyto scénáře patří:</span><span class="sxs-lookup"><span data-stu-id="086fc-105">Some examples of those scenarios are:</span></span>

- <span data-ttu-id="086fc-106">Cluster Service Fabric zahrnuje geografické vzdálenosti, například několik datových center v místě nebo mezi oblastmi Azure</span><span class="sxs-lookup"><span data-stu-id="086fc-106">Your Service Fabric cluster spans geographic distances, such as multiple on-premises datacenters or across Azure regions</span></span>
- <span data-ttu-id="086fc-107">Vaše prostředí zahrnuje více oblastí řízení geopolitické nebo právních nebo některých jiných případ, kdy máte zásady hranice je nutné tooenforce</span><span class="sxs-lookup"><span data-stu-id="086fc-107">Your environment spans multiple areas of geopolitical or legal control, or some other case where you have policy boundaries you need tooenforce</span></span>
- <span data-ttu-id="086fc-108">Komunikace výkon nebo latenci rozhodnutí z důvodu toolarge vzdálenosti nebo použití pomalejší nebo méně spolehlivé síťová propojení</span><span class="sxs-lookup"><span data-stu-id="086fc-108">There are communication performance or latency considerations due toolarge distances or use of slower or less reliable network links</span></span>
- <span data-ttu-id="086fc-109">Je třeba tookeep určité úlohy společně umístěná jako usilovně se s ostatními úlohami nebo v blízkosti toocustomers</span><span class="sxs-lookup"><span data-stu-id="086fc-109">You need tookeep certain workloads collocated as a best effort, either with other workloads or in proximity toocustomers</span></span>

<span data-ttu-id="086fc-110">Většinu těchto požadavků zarovnané s hello fyzické rozložení hello clusteru, reprezentována jako hello domén selhání clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="086fc-110">Most of these requirements align with hello physical layout of hello cluster, represented as hello fault domains of hello cluster.</span></span> 

<span data-ttu-id="086fc-111">Hello pokročilé zásady umístění, které pomáhají adresu, kterou jsou tyto scénáře:</span><span class="sxs-lookup"><span data-stu-id="086fc-111">hello advanced placement policies that help address these scenarios are:</span></span>

1. <span data-ttu-id="086fc-112">Neplatný domén</span><span class="sxs-lookup"><span data-stu-id="086fc-112">Invalid domains</span></span>
2. <span data-ttu-id="086fc-113">Požadované domén</span><span class="sxs-lookup"><span data-stu-id="086fc-113">Required domains</span></span>
3. <span data-ttu-id="086fc-114">Upřednostňovaných domén</span><span class="sxs-lookup"><span data-stu-id="086fc-114">Preferred domains</span></span>
4. <span data-ttu-id="086fc-115">Zakazuje se okolních repliky</span><span class="sxs-lookup"><span data-stu-id="086fc-115">Disallowing replica packing</span></span>

<span data-ttu-id="086fc-116">Většina hello následující ovládací prvky se daly konfigurovat prostřednictvím vlastnosti uzlu a omezení umístění, ale některé jsou složitější.</span><span class="sxs-lookup"><span data-stu-id="086fc-116">Most of hello following controls could be configured via node properties and placement constraints, but some are more complicated.</span></span> <span data-ttu-id="086fc-117">věcí toomake jednodušší, hello správce prostředků clusteru Service Fabric nabízí tyto zásady další umístění.</span><span class="sxs-lookup"><span data-stu-id="086fc-117">toomake things simpler, hello Service Fabric Cluster Resource Manager provides these additional placement policies.</span></span> <span data-ttu-id="086fc-118">Konfigurace zásad umístění na základě služby za názvem instance.</span><span class="sxs-lookup"><span data-stu-id="086fc-118">Placement policies are configured on a per-named service instance basis.</span></span> <span data-ttu-id="086fc-119">Se dá se taky aktualizovat dynamicky.</span><span class="sxs-lookup"><span data-stu-id="086fc-119">They can also be updated dynamically.</span></span>

## <a name="specifying-invalid-domains"></a><span data-ttu-id="086fc-120">Neplatné zadání domén</span><span class="sxs-lookup"><span data-stu-id="086fc-120">Specifying invalid domains</span></span>
<span data-ttu-id="086fc-121">Hello **InvalidDomain** umístění zásad vám umožní toospecify, že je neplatný pro konkrétní službu konkrétní domény selhání.</span><span class="sxs-lookup"><span data-stu-id="086fc-121">hello **InvalidDomain** placement policy allows you toospecify that a particular Fault Domain is invalid for a specific service.</span></span> <span data-ttu-id="086fc-122">Tato zásada zajišťuje, že konkrétní službu nikdy běží v určité oblasti, například důvodů geopolitické nebo podnikové zásady.</span><span class="sxs-lookup"><span data-stu-id="086fc-122">This policy ensures that a particular service never runs in a particular area, for example for geopolitical or corporate policy reasons.</span></span> <span data-ttu-id="086fc-123">Přes samostatné zásady je možné zadat více domén neplatný.</span><span class="sxs-lookup"><span data-stu-id="086fc-123">Multiple invalid domains may be specified via separate policies.</span></span>

<span data-ttu-id="086fc-124"><center>
![Příklad domény je neplatný][Image1]
</center></span><span class="sxs-lookup"><span data-stu-id="086fc-124"><center>
![Invalid Domain Example][Image1]
</center></span></span>

<span data-ttu-id="086fc-125">Kód:</span><span class="sxs-lookup"><span data-stu-id="086fc-125">Code:</span></span>

```csharp
ServicePlacementInvalidDomainPolicyDescription invalidDomain = new ServicePlacementInvalidDomainPolicyDescription();
invalidDomain.DomainName = "fd:/DCEast"; //regulations prohibit this workload here
serviceDescription.PlacementPolicies.Add(invalidDomain);
```

<span data-ttu-id="086fc-126">Prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="086fc-126">Powershell:</span></span>

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("InvalidDomain,fd:/DCEast”)
```
## <a name="specifying-required-domains"></a><span data-ttu-id="086fc-127">Určení požadované domén</span><span class="sxs-lookup"><span data-stu-id="086fc-127">Specifying required domains</span></span>
<span data-ttu-id="086fc-128">Hello požadované zásady umístění domény vyžaduje, aby služba hello pouze v zadané doméně hello.</span><span class="sxs-lookup"><span data-stu-id="086fc-128">hello required domain placement policy requires that hello service is present only in hello specified domain.</span></span> <span data-ttu-id="086fc-129">Lze zadat více domén požadované přes samostatné zásady.</span><span class="sxs-lookup"><span data-stu-id="086fc-129">Multiple required domains can be specified via separate policies.</span></span>

<span data-ttu-id="086fc-130"><center>
![Příklad požadovanou doménu][Image2]
</center></span><span class="sxs-lookup"><span data-stu-id="086fc-130"><center>
![Required Domain Example][Image2]
</center></span></span>

<span data-ttu-id="086fc-131">Kód:</span><span class="sxs-lookup"><span data-stu-id="086fc-131">Code:</span></span>

```csharp
ServicePlacementRequiredDomainPolicyDescription requiredDomain = new ServicePlacementRequiredDomainPolicyDescription();
requiredDomain.DomainName = "fd:/DC01/RK03/BL2";
serviceDescription.PlacementPolicies.Add(requiredDomain);
```

<span data-ttu-id="086fc-132">Prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="086fc-132">Powershell:</span></span>

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("RequiredDomain,fd:/DC01/RK03/BL2")
```

## <a name="specifying-a-preferred-domain-for-hello-primary-replicas-of-a-stateful-service"></a><span data-ttu-id="086fc-133">Určení upřednostňované domény pro primární repliky hello stavové služby</span><span class="sxs-lookup"><span data-stu-id="086fc-133">Specifying a preferred domain for hello primary replicas of a stateful service</span></span>
<span data-ttu-id="086fc-134">Hello preferované primární doménu určuje tooplace domény selhání hello hello primární v.</span><span class="sxs-lookup"><span data-stu-id="086fc-134">hello Preferred Primary Domain specifies hello fault domain tooplace hello Primary in.</span></span> <span data-ttu-id="086fc-135">Hello primární skončilo v této doméně při všechno, co je v pořádku.</span><span class="sxs-lookup"><span data-stu-id="086fc-135">hello Primary ends up in this domain when everything is healthy.</span></span> <span data-ttu-id="086fc-136">Pokud hello domény nebo hello primární repliky selže nebo vypne, hello primární přesune toosome jiného umístění, nejlépe ve hello stejné domény.</span><span class="sxs-lookup"><span data-stu-id="086fc-136">If hello domain or hello Primary replica fails or shuts down, hello Primary moves toosome other location, ideally in hello same domain.</span></span> <span data-ttu-id="086fc-137">Pokud toto nové umístění není v doméně upřednostňované hello, hello správce prostředků clusteru přesune zpátky upřednostňované domény toohello co nejdříve.</span><span class="sxs-lookup"><span data-stu-id="086fc-137">If this new location isn't in hello preferred domain, hello Cluster Resource Manager moves it back toohello preferred domain as soon as possible.</span></span> <span data-ttu-id="086fc-138">Přirozeně toto nastavení má smysl jenom pro stavové služby.</span><span class="sxs-lookup"><span data-stu-id="086fc-138">Naturally this setting only makes sense for stateful services.</span></span> <span data-ttu-id="086fc-139">Tato zásada je velmi užitečné v clusterech, které jsou rozloženy oblastí Azure nebo několik datových center ale máte služby, které dávají přednost umístění v určitých umístění.</span><span class="sxs-lookup"><span data-stu-id="086fc-139">This policy is most useful in clusters that are spanned across Azure regions or multiple datacenters but have services that prefer placement in a certain location.</span></span> <span data-ttu-id="086fc-140">Základní udržet barvy zavřete tootheir uživatele nebo jiné služby, které pomáhá poskytovat nižší latenci, hlavně pro čtení, které jsou zpracovávány základní barvy ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="086fc-140">Keeping Primaries close tootheir users or other services helps provide lower latency, especially for reads, which are handled by Primaries by default.</span></span>

<span data-ttu-id="086fc-141"><center>
![Upřednostňovaný primární domény a převzetí služeb při selhání][Image3]
</center></span><span class="sxs-lookup"><span data-stu-id="086fc-141"><center>
![Preferred Primary Domains and Failover][Image3]
</center></span></span>

```csharp
ServicePlacementPreferPrimaryDomainPolicyDescription primaryDomain = new ServicePlacementPreferPrimaryDomainPolicyDescription();
primaryDomain.DomainName = "fd:/EastUS/";
serviceDescription.PlacementPolicies.Add(invalidDomain);
```

<span data-ttu-id="086fc-142">Prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="086fc-142">Powershell:</span></span>

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("PreferredPrimaryDomain,fd:/EastUS")
```

## <a name="requiring-replica-distribution-and-disallowing-packing"></a><span data-ttu-id="086fc-143">Vyžadování distribučních repliky a zakazuje okolních</span><span class="sxs-lookup"><span data-stu-id="086fc-143">Requiring replica distribution and disallowing packing</span></span>
<span data-ttu-id="086fc-144">Repliky jsou _normálně_ distribuován napříč doménami selhání a upgradu, když je v pořádku hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="086fc-144">Replicas are _normally_ distributed across fault and upgrade domains when hello cluster is healthy.</span></span> <span data-ttu-id="086fc-145">Existují však případy, kdy může se stát víc než jednu repliku pro daný oddíl dočasně sbalené do jedné domény.</span><span class="sxs-lookup"><span data-stu-id="086fc-145">However, there are cases where more than one replica for a given partition may end up temporarily packed into a single domain.</span></span> <span data-ttu-id="086fc-146">Předpokládejme například, že tento cluster hello má devět uzly v tři domén selhání, fd: / 0, fd: / 1 a fd: / 2.</span><span class="sxs-lookup"><span data-stu-id="086fc-146">For example, let's say that hello cluster has nine nodes in three fault domains, fd:/0, fd:/1, and fd:/2.</span></span> <span data-ttu-id="086fc-147">Dále předpokládejme, že vaše služba má tři repliky.</span><span class="sxs-lookup"><span data-stu-id="086fc-147">Let's also say that your service has three replicas.</span></span> <span data-ttu-id="086fc-148">Řekněme, že hello uzly, které byly používány pro tyto repliky v fd: / 1 a fd: / 2 byl vypnut.</span><span class="sxs-lookup"><span data-stu-id="086fc-148">Let's say that hello nodes that were being used for those replicas in fd:/1 and fd:/2 went down.</span></span> <span data-ttu-id="086fc-149">Za normálních okolností hello správce prostředků clusteru přejete jiné uzly v těchto stejné domény selhání.</span><span class="sxs-lookup"><span data-stu-id="086fc-149">Normally hello Cluster Resource Manager would prefer other nodes in those same fault domains.</span></span> <span data-ttu-id="086fc-150">V takovém případě Řekněme z důvodu problémů toocapacity žádné jiné uzly v těchto doménách hello byl platný.</span><span class="sxs-lookup"><span data-stu-id="086fc-150">In this case, let's say due toocapacity issues none of hello other nodes in those domains were valid.</span></span> <span data-ttu-id="086fc-151">Pokud hello správce prostředků clusteru sestavení nahrazení pro tyto repliky, se bude mít toochoose uzly v fd: / 0.</span><span class="sxs-lookup"><span data-stu-id="086fc-151">If hello Cluster Resource Manager builds replacements for those replicas, it would have toochoose nodes in fd:/0.</span></span> <span data-ttu-id="086fc-152">To však _,_ vytvoří situaci porušena hello omezení domény selhání.</span><span class="sxs-lookup"><span data-stu-id="086fc-152">However, doing _that_ creates a situation where hello Fault Domain constraint is violated.</span></span> <span data-ttu-id="086fc-153">Balení repliky zvyšuje hello pravděpodobnost, že hello sady replik celou může přejděte a ztratí.</span><span class="sxs-lookup"><span data-stu-id="086fc-153">Packing replicas increases hello chance that hello whole replica set could go down or be lost.</span></span> 

> [!NOTE]
> <span data-ttu-id="086fc-154">Další informace o omezení a omezení priority obecně platí, podívejte se na [v tomto tématu](service-fabric-cluster-resource-manager-management-integration.md#constraint-priorities).</span><span class="sxs-lookup"><span data-stu-id="086fc-154">For more information on constraints and constraint priorities generally, check out [this topic](service-fabric-cluster-resource-manager-management-integration.md#constraint-priorities).</span></span>
>

<span data-ttu-id="086fc-155">Pokud někdy vidíte zprávu stavu, jako "`hello Load Balancer has detected a Constraint Violation for this Replica:fabric:/<some service name> Secondary Partition <some partition ID> is violating hello Constraint: FaultDomain`", pak jste stisknutí tlačítka tato podmínka nebo něco podobného jako ho.</span><span class="sxs-lookup"><span data-stu-id="086fc-155">If you've ever seen a health message such as "`hello Load Balancer has detected a Constraint Violation for this Replica:fabric:/<some service name> Secondary Partition <some partition ID> is violating hello Constraint: FaultDomain`", then you've hit this condition or something like it.</span></span> <span data-ttu-id="086fc-156">Obvykle pouze jednu nebo dvě repliky jsou společně zabaleny dočasně.</span><span class="sxs-lookup"><span data-stu-id="086fc-156">Usually only one or two replicas are packed together temporarily.</span></span> <span data-ttu-id="086fc-157">Tak dlouho, dokud je méně než kvorum repliky v dané doméně, jste bezpečné.</span><span class="sxs-lookup"><span data-stu-id="086fc-157">So long as there are fewer than a quorum of replicas in a given domain, you're safe.</span></span> <span data-ttu-id="086fc-158">Okolních je taková situace vzácná, ale může dojít a obvykle jsou tyto situace přechodný vzhledem k tomu, že uzly hello vraťte.</span><span class="sxs-lookup"><span data-stu-id="086fc-158">Packing is rare, but it can happen, and usually these situations are transient since hello nodes come back.</span></span> <span data-ttu-id="086fc-159">Pokud uzly hello zůstat dolů a hello správce prostředků clusteru musí toobuild náhrady, obvykle existuje jiné uzly v doménách selhání ideální hello.</span><span class="sxs-lookup"><span data-stu-id="086fc-159">If hello nodes do stay down and hello Cluster Resource Manager needs toobuild replacements, usually there are other nodes available in hello ideal fault domains.</span></span>

<span data-ttu-id="086fc-160">Některé úlohy by raději vždy s hello cílový počet replik, i když jsou zabaleny do menšího počtu domén.</span><span class="sxs-lookup"><span data-stu-id="086fc-160">Some workloads would prefer always having hello target number of replicas, even if they are packed into fewer domains.</span></span> <span data-ttu-id="086fc-161">Tyto úlohy jsou ale proti selhání celkový počet souběžných trvalé domény a obvykle můžete obnovit místní stavu.</span><span class="sxs-lookup"><span data-stu-id="086fc-161">These workloads are betting against total simultaneous permanent domain failures and can usually recover local state.</span></span> <span data-ttu-id="086fc-162">Další úlohy by starší než riziko správnost nebo ke ztrátě dat. místo trvat hello výpadku.</span><span class="sxs-lookup"><span data-stu-id="086fc-162">Other workloads would rather take hello downtime earlier than risk correctness or loss of data.</span></span> <span data-ttu-id="086fc-163">Většina produkční úlohy spustit s víc než tři repliky, víc než třemi domén selhání a mnoho platný uzlů na domény selhání.</span><span class="sxs-lookup"><span data-stu-id="086fc-163">Most production workloads run with more than three replicas, more than three fault domains, and many valid nodes per fault domain.</span></span> <span data-ttu-id="086fc-164">Z toho důvodu hello výchozí chování umožňuje okolních domény ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="086fc-164">Because of this, hello default behavior allows domain packing by default.</span></span> <span data-ttu-id="086fc-165">Hello výchozí chování umožňuje normální vyrovnávání a převzetí služeb při selhání toohandle tyto výjimečných případech i v případě, že to znamená okolních dočasné domény.</span><span class="sxs-lookup"><span data-stu-id="086fc-165">hello default behavior allows normal balancing and failover toohandle these extreme cases, even if that means temporary domain packing.</span></span>

<span data-ttu-id="086fc-166">Pokud chcete toodisable těchto okolních pro danou úlohu, můžete zadat hello `RequireDomainDistribution` zásady ve službě hello.</span><span class="sxs-lookup"><span data-stu-id="086fc-166">If you want toodisable such packing for a given workload, you can specify hello `RequireDomainDistribution` policy on hello service.</span></span> <span data-ttu-id="086fc-167">Pokud je tato zásada nastavená, hello správce prostředků clusteru zajišťuje žádné dvě repliky ze stejného oddílu spustit v hello stejné poruch nebo upgradu domény hello.</span><span class="sxs-lookup"><span data-stu-id="086fc-167">When this policy is set, hello Cluster Resource Manager ensures no two replicas from hello same partition run in hello same fault or upgrade domain.</span></span>

<span data-ttu-id="086fc-168">Kód:</span><span class="sxs-lookup"><span data-stu-id="086fc-168">Code:</span></span>

```csharp
ServicePlacementRequireDomainDistributionPolicyDescription distributeDomain = new ServicePlacementRequireDomainDistributionPolicyDescription();
serviceDescription.PlacementPolicies.Add(distributeDomain);
```

<span data-ttu-id="086fc-169">Prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="086fc-169">Powershell:</span></span>

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementPolicy @("RequiredDomainDistribution")
```

<span data-ttu-id="086fc-170">Teď, jej by být možné toouse tyto konfigurace pro služby v clusteru, který nebyl předané geograficky?</span><span class="sxs-lookup"><span data-stu-id="086fc-170">Now, would it be possible toouse these configurations for services in a cluster that was not geographically spanned?</span></span> <span data-ttu-id="086fc-171">Vám může, ale není skvělé důvod příliš.</span><span class="sxs-lookup"><span data-stu-id="086fc-171">You could, but there’s not a great reason too.</span></span> <span data-ttu-id="086fc-172">Hello konfigurace vyžaduje, neplatný a upřednostňované domény je nutno Pokud hello scénáře, které je vyžadují.</span><span class="sxs-lookup"><span data-stu-id="086fc-172">hello required, invalid, and preferred domain configurations should be avoided unless hello scenarios require them.</span></span> <span data-ttu-id="086fc-173">Ho neznamená, že všechny smysl tootry tooforce toorun daného zatížení v jedné rack nebo tooprefer některé segment místním clusteru oproti jinému.</span><span class="sxs-lookup"><span data-stu-id="086fc-173">It doesn't make any sense tootry tooforce a given workload toorun in a single rack, or tooprefer some segment of your local cluster over another.</span></span> <span data-ttu-id="086fc-174">Různé hardwarové konfigurace by měl být rozloženy domén selhání a zpracovávají pomocí normální umístění omezení a vlastnosti uzlu.</span><span class="sxs-lookup"><span data-stu-id="086fc-174">Different hardware configurations should be spread across fault domains and handled via normal placement constraints and node properties.</span></span>

## <a name="next-steps"></a><span data-ttu-id="086fc-175">Další kroky</span><span class="sxs-lookup"><span data-stu-id="086fc-175">Next steps</span></span>
- <span data-ttu-id="086fc-176">Další informace o konfiguraci služby [Další informace o konfiguraci služby](service-fabric-cluster-resource-manager-configure-services.md)</span><span class="sxs-lookup"><span data-stu-id="086fc-176">For more information on configuring services, [Learn about configuring Services](service-fabric-cluster-resource-manager-configure-services.md)</span></span>

[Image1]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies/cluster-invalid-placement-domain.png
[Image2]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies/cluster-required-placement-domain.png
[Image3]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies/cluster-preferred-primary-domain.png
