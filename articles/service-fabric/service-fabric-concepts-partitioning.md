---
title: "Vytváření oddílů služby Service Fabric | Microsoft Docs"
description: "Popisuje, jak při vytváření oddílů stavové služby Service Fabric. Oddíly umožňuje ukládání dat na místní počítače, data a výpočetní je možné rozšířit společně."
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: 
ms.assetid: 3b7248c8-ea92-4964-85e7-6f1291b5cc7b
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/30/2017
ms.author: msfussell
ms.openlocfilehash: 3c1e80305cb65f41a6981b99f69e8b87f89599ac
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="partition-service-fabric-reliable-services"></a><span data-ttu-id="f0f0e-104">Spolehlivé služby oddílu Service Fabric</span><span class="sxs-lookup"><span data-stu-id="f0f0e-104">Partition Service Fabric reliable services</span></span>
<span data-ttu-id="f0f0e-105">Tento článek obsahuje úvod do vytváření oddílů spolehlivé služby Azure Service Fabric se základními koncepty.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-105">This article provides an introduction to the basic concepts of partitioning Azure Service Fabric reliable services.</span></span> <span data-ttu-id="f0f0e-106">Zdrojový kód používá v článku je také k dispozici na [Githubu](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/classic/Services/AlphabetPartitions).</span><span class="sxs-lookup"><span data-stu-id="f0f0e-106">The source code used in the article is also available on [GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/classic/Services/AlphabetPartitions).</span></span>

## <a name="partitioning"></a><span data-ttu-id="f0f0e-107">Dělení</span><span class="sxs-lookup"><span data-stu-id="f0f0e-107">Partitioning</span></span>
<span data-ttu-id="f0f0e-108">Dělení na oddíly není jedinečný pro Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-108">Partitioning is not unique to Service Fabric.</span></span> <span data-ttu-id="f0f0e-109">Ve skutečnosti je základní vzor vytváření škálovatelných služeb.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-109">In fact, it is a core pattern of building scalable services.</span></span> <span data-ttu-id="f0f0e-110">V širším významu jsme vezměte v úvahu dělení jako koncept dělení stavu (data) a výpočetní do menších přístupné jednotky pro zlepšení škálovatelnosti a výkonu.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-110">In a broader sense, we can think about partitioning as a concept of dividing state (data) and compute into smaller accessible units to improve scalability and performance.</span></span> <span data-ttu-id="f0f0e-111">Dobře známé formulář oddíly je [vytvoření oddílů dat][wikipartition], označované také jako horizontálního dělení.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-111">A well-known form of partitioning is [data partitioning][wikipartition], also known as sharding.</span></span>

### <a name="partition-service-fabric-stateless-services"></a><span data-ttu-id="f0f0e-112">Bezstavové služby oddílu Service Fabric</span><span class="sxs-lookup"><span data-stu-id="f0f0e-112">Partition Service Fabric stateless services</span></span>
<span data-ttu-id="f0f0e-113">Pro bezstavové služby si myslíte o oddílu se logická jednotka, která obsahuje jeden nebo více instancí služby.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-113">For stateless services, you can think about a partition being a logical unit that contains one or more instances of a service.</span></span> <span data-ttu-id="f0f0e-114">Obrázek 1 zobrazuje bezstavové služby s pět instancí, které jsou rozmístěny v clusteru pomocí jeden oddíl.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-114">Figure 1 shows a stateless service with five instances distributed across a cluster using one partition.</span></span>

![Bezstavové služby](./media/service-fabric-concepts-partitioning/statelessinstances.png)

<span data-ttu-id="f0f0e-116">Skutečně existují dva typy řešení bezstavové služby.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-116">There are really two types of stateless service solutions.</span></span> <span data-ttu-id="f0f0e-117">První z nich je služba, která přetrvává stav externě, například v Azure SQL database (jako je web, který ukládá informace o relaci a data).</span><span class="sxs-lookup"><span data-stu-id="f0f0e-117">The first one is a service that persists its state externally, for example in an Azure SQL database (like a website that stores the session information and data).</span></span> <span data-ttu-id="f0f0e-118">Druhá je jen výpočetní služby (např. kalkulačky nebo bitové kopie vytváření miniatur), které nespravujete žádné trvalý stav.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-118">The second one is computation-only services (like a calculator or image thumbnailing) that do not manage any persistent state.</span></span>

<span data-ttu-id="f0f0e-119">Buď v případech dělení bezstavové služby je velmi výjimečných scénář--škálovatelnost a dostupnosti jsou obvykle dosáhnete přidáním více instancí.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-119">In either case, partitioning a stateless service is a very rare scenario--scalability and availability are normally achieved by adding more instances.</span></span> <span data-ttu-id="f0f0e-120">Pouze když budete chtít zvážit více oddílů pro bezstavové služby instance je, když je třeba splňovat speciální požadavky na směrování.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-120">The only time you want to consider multiple partitions for stateless service instances is when you need to meet special routing requests.</span></span>

<span data-ttu-id="f0f0e-121">Jako příklad Představte si případ, kdy uživatelé s identifikátory v určitý rozsah má být zpracován pouze instance konkrétní službu.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-121">As an example, consider a case where users with IDs in a certain range should only be served by a particular service instance.</span></span> <span data-ttu-id="f0f0e-122">Další příklad, kdy může oddílu bezstavové služby je, pokud máte skutečně oddílů back-end (např. horizontálně dělené databáze SQL) a chcete určovat, které instanci služby musí zapsat do databáze horizontálních – nebo můžete provádět jinou práci přípravy v rámci bezstavové služby, který vyžaduje stejné rozdělení informace, jak se používá v back-end.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-122">Another example of when you could partition a stateless service is when you have a truly partitioned backend (e.g. a sharded SQL database) and you want to control which service instance should write to the database shard--or perform other preparation work within the stateless service that requires the same partitioning information as is used in the backend.</span></span> <span data-ttu-id="f0f0e-123">Tyto typy scénáře lze vyřešit různými způsoby a nevyžadují nutně rozdělení do oddílů služby.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-123">Those types of scenarios can also be solved in different ways and do not necessarily require service partitioning.</span></span>

<span data-ttu-id="f0f0e-124">Zbývající část tohoto návodu se zaměřuje na stavové služby.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-124">The remainder of this walkthrough focuses on stateful services.</span></span>

### <a name="partition-service-fabric-stateful-services"></a><span data-ttu-id="f0f0e-125">Oddíl Service Fabric stavové služby</span><span class="sxs-lookup"><span data-stu-id="f0f0e-125">Partition Service Fabric stateful services</span></span>
<span data-ttu-id="f0f0e-126">Service Fabric usnadňuje vývoj škálovatelných služeb stavová tím, že nabízí prvotřídní způsob, jak stav oddílu (data).</span><span class="sxs-lookup"><span data-stu-id="f0f0e-126">Service Fabric makes it easy to develop scalable stateful services by offering a first-class way to partition state (data).</span></span> <span data-ttu-id="f0f0e-127">Koncepčně, si můžete představit o oddílu stavové služby jako jednotku škálování, která je vysoce spolehlivé prostřednictvím [repliky](service-fabric-availability-services.md) které jsou distribuované a rovnoměrně rozdělen mezi uzly v clusteru.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-127">Conceptually, you can think about a partition of a stateful service as a scale unit that is highly reliable through [replicas](service-fabric-availability-services.md) that are distributed and balanced across the nodes in a cluster.</span></span>

<span data-ttu-id="f0f0e-128">Vytváření oddílů v kontextu Service Fabric stavové služby odkazuje na proces pro určení toho, že oddíl konkrétní službu je zodpovědná za část dokončení stav služby.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-128">Partitioning in the context of Service Fabric stateful services refers to the process of determining that a particular service partition is responsible for a portion of the complete state of the service.</span></span> <span data-ttu-id="f0f0e-129">(Jak je uvedeno nahoře, oddíl je sada [repliky](service-fabric-availability-services.md)).</span><span class="sxs-lookup"><span data-stu-id="f0f0e-129">(As mentioned before, a partition is a set of [replicas](service-fabric-availability-services.md)).</span></span> <span data-ttu-id="f0f0e-130">Skvělé věc o Service Fabric se umístí oddíly v různých uzlech.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-130">A great thing about Service Fabric is that it places the partitions on different nodes.</span></span> <span data-ttu-id="f0f0e-131">To umožňuje jejich dosáhnout limitu prostředků uzlu.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-131">This allows them to grow to a node's resource limit.</span></span> <span data-ttu-id="f0f0e-132">Jako data potřebuje růst, oddíly růst a Service Fabric oddíly znovu vytvoří rovnováhu mezi uzly.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-132">As the data needs grow, partitions grow, and Service Fabric rebalances partitions across nodes.</span></span> <span data-ttu-id="f0f0e-133">Tím se zajistí nepřetržitou efektivní využití hardwarových prostředků.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-133">This ensures the continued efficient use of hardware resources.</span></span>

<span data-ttu-id="f0f0e-134">Tak, abyste získali příklad, stát, že začínáte s 5 uzly clusteru a služby, který je nakonfigurovaný tak, aby měl 10 oddílů a cílem tři repliky.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-134">To give you an example, say you start with a 5-node cluster and a service that is configured to have 10 partitions and a target of three replicas.</span></span> <span data-ttu-id="f0f0e-135">V takovém případě by vyvážit a distribuovat repliky do clusteru--Service Fabric a skončili byste s dva primární [repliky](service-fabric-availability-services.md) na jeden uzel.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-135">In this case, Service Fabric would balance and distribute the replicas across the cluster--and you would end up with two primary [replicas](service-fabric-availability-services.md) per node.</span></span>
<span data-ttu-id="f0f0e-136">Pokud potřebujete škálování clusteru 10 uzly, Service Fabric by znovu vyvážit primární [repliky](service-fabric-availability-services.md) pro všechny uzly 10.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-136">If you now need to scale out the cluster to 10 nodes, Service Fabric would rebalance the primary [replicas](service-fabric-availability-services.md) across all 10 nodes.</span></span> <span data-ttu-id="f0f0e-137">Podobně při změně měřítka zpět do 5 uzly Service Fabric by znovu vyvážit všechny repliky mezi 5 uzly.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-137">Likewise, if you scaled back to 5 nodes, Service Fabric would rebalance all the replicas across the 5 nodes.</span></span>  

<span data-ttu-id="f0f0e-138">Obrázek 2 ukazuje rozdělení 10 oddílů před a po škálování clusteru.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-138">Figure 2 shows the distribution of 10 partitions before and after scaling the cluster.</span></span>

![Stavové služby](./media/service-fabric-concepts-partitioning/partitions.png)

<span data-ttu-id="f0f0e-140">V důsledku toho Škálováním na více systémů se dosáhne vzhledem k tomu, že požadavky od klientů, které jsou rozmístěny v počítačích, celkový výkon aplikace je vyšší, a tím se snižuje kolize na přístup k bloky dat s.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-140">As a result, the scale-out is achieved since requests from clients are distributed across computers, overall performance of the application is improved, and contention on access to chunks of data is reduced.</span></span>

## <a name="plan-for-partitioning"></a><span data-ttu-id="f0f0e-141">Plán pro dělení</span><span class="sxs-lookup"><span data-stu-id="f0f0e-141">Plan for partitioning</span></span>
<span data-ttu-id="f0f0e-142">Před implementací služby, byste měli zvážit vždy rozdělení strategii, která je potřeba horizontální navýšení kapacity.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-142">Before implementing a service, you should always consider the partitioning strategy that is required to scale out.</span></span> <span data-ttu-id="f0f0e-143">Existují různé způsoby, ale všechny z nich zaměřit se na aplikace potřebuje k dosažení.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-143">There are different ways, but all of them focus on what the application needs to achieve.</span></span> <span data-ttu-id="f0f0e-144">V kontextu tohoto článku zvažte některé důležité aspekty.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-144">For the context of this article, let's consider some of the more important aspects.</span></span>

<span data-ttu-id="f0f0e-145">Dobrým přístupem je myslíte o struktuře stavu, který musí být dělené jako první krok.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-145">A good approach is to think about the structure of the state that needs to be partitioned, as the first step.</span></span>

<span data-ttu-id="f0f0e-146">Podívejme jednoduchý příklad.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-146">Let's take a simple example.</span></span> <span data-ttu-id="f0f0e-147">Pokud byste chtěli vytvořit službu pro countywide dotazování, můžete vytvořit oddíl pro každé město v kraj.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-147">If you were to build a service for a countywide poll, you could create a partition for each city in the county.</span></span> <span data-ttu-id="f0f0e-148">Potom může ukládat hlasy pro každou osobu v městě v oddílu, který odpovídá této města.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-148">Then, you could store the votes for every person in the city in the partition that corresponds to that city.</span></span> <span data-ttu-id="f0f0e-149">Obrázek 3 znázorňuje sadu osoby a města, ve kterém se nacházejí.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-149">Figure 3 illustrates a set of people and the city in which they reside.</span></span>

![Jednoduché oddílu](./media/service-fabric-concepts-partitioning/cities.png)

<span data-ttu-id="f0f0e-151">Jako naplnění města se výrazně liší, můžete skončit s některé oddíly, které obsahují velké množství dat (např. Praha) a ostatní oddíly s velmi malé stavu (např. Kirkland).</span><span class="sxs-lookup"><span data-stu-id="f0f0e-151">As the population of cities varies widely, you may end up with some partitions that contain a lot of data (e.g. Seattle) and other partitions with very little state (e.g. Kirkland).</span></span> <span data-ttu-id="f0f0e-152">A co je dopad s oddíly s nerovnoměrné objemy stavu?</span><span class="sxs-lookup"><span data-stu-id="f0f0e-152">So what is the impact of having partitions with uneven amounts of state?</span></span>

<span data-ttu-id="f0f0e-153">Pokud přemýšlíte o příklad znovu, snadno uvidíte, že oddílu, který obsahuje hlasy pro prahu získáte více požadavků než Kirkland jeden.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-153">If you think about the example again, you can easily see that the partition that holds the votes for Seattle will get more traffic than the Kirkland one.</span></span> <span data-ttu-id="f0f0e-154">Ve výchozím nastavení vytvoří se, že je o stejný počet primární a sekundární repliky na každém uzlu Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-154">By default, Service Fabric makes sure that there is about the same number of primary and secondary replicas on each node.</span></span> <span data-ttu-id="f0f0e-155">Proto můžete skončit s uzly, které mají replik, které poskytovat další provoz a ostatní, které slouží menší provoz.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-155">So you may end up with nodes that hold replicas that serve more traffic and others that serve less traffic.</span></span> <span data-ttu-id="f0f0e-156">Chcete by pokud možno se vyhnout úrovněmi horkého a studeného body takto v clusteru.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-156">You would preferably want to avoid hot and cold spots like this in a cluster.</span></span>

<span data-ttu-id="f0f0e-157">Chcete-li předejít, měli byste udělat dvě věci, z hlediska rozdělení oddílů:</span><span class="sxs-lookup"><span data-stu-id="f0f0e-157">In order to avoid this, you should do two things, from a partitioning point of view:</span></span>

* <span data-ttu-id="f0f0e-158">Pokuste se stav oddílu, tak, aby je rovnoměrně rozdělené mezi všechny oddíly.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-158">Try to partition the state so that it is evenly distributed across all partitions.</span></span>
* <span data-ttu-id="f0f0e-159">Načtení sestav z jednotlivé repliky pro službu.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-159">Report load from each of the replicas for the service.</span></span> <span data-ttu-id="f0f0e-160">(Informace o tom, projděte si tento článek na [metriky a zatížení](service-fabric-cluster-resource-manager-metrics.md)).</span><span class="sxs-lookup"><span data-stu-id="f0f0e-160">(For information on how, check out this article on [Metrics and Load](service-fabric-cluster-resource-manager-metrics.md)).</span></span> <span data-ttu-id="f0f0e-161">Service Fabric nabízí možnosti sestavy zatížení spotřebovávají služby, jako je množství paměti nebo počet záznamů.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-161">Service Fabric provides the capability to report load consumed by services, such as amount of memory or number of records.</span></span> <span data-ttu-id="f0f0e-162">Na základě metrik hlášené, Service Fabric zjistí, že některé oddíly slouží větší objemy než jiné a znovu vytvoří rovnováhu clusteru přesunutím repliky pro vhodnější uzly, takže celkový je přetížena žádný uzel.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-162">Based on the metrics reported, Service Fabric detects that some partitions are serving higher loads than others and rebalances the cluster by moving replicas to more suitable nodes, so that overall no node is overloaded.</span></span>

<span data-ttu-id="f0f0e-163">V některých případech nelze vědět, kolik dat bude v daném oddílu.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-163">Sometimes, you cannot know how much data will be in a given partition.</span></span> <span data-ttu-id="f0f0e-164">Obecné doporučení je oba – nejprve přijetím strategie dělení který se šíří data rovnoměrně mezi oddílů a druhý, pomocí sestav zatížení.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-164">So a general recommendation is to do both--first, by adopting a partitioning strategy that spreads the data evenly across the partitions and second, by reporting load.</span></span>  <span data-ttu-id="f0f0e-165">První metoda zabraňuje případů popsaných v hlasování příkladu při druhý pomáhá vyhlazení dočasné rozdíly v přístupu nebo zatížení v čase.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-165">The first method prevents situations described in the voting example, while the second helps smooth out temporary differences in access or load over time.</span></span>

<span data-ttu-id="f0f0e-166">Další aspekt plánování oddílu je vybrat správný počet oddílů na začátku.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-166">Another aspect of partition planning is to choose the correct number of partitions to begin with.</span></span>
<span data-ttu-id="f0f0e-167">Z hlediska Service Fabric není nic, které zabraňují začínáte s vyšší počet oddílů, než se očekává pro váš scénář.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-167">From a Service Fabric perspective, there is nothing that prevents you from starting out with a higher number of partitions than anticipated for your scenario.</span></span>
<span data-ttu-id="f0f0e-168">Za předpokladu, že maximální počet oddílů ve skutečnosti je platný přístup.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-168">In fact, assuming the maximum number of partitions is a valid approach.</span></span>

<span data-ttu-id="f0f0e-169">Ve výjimečných případech můžete dospět nutnosti více oddílů, než jste původně vybrali.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-169">In rare cases, you may end up needing more partitions than you have initially chosen.</span></span> <span data-ttu-id="f0f0e-170">Jako počet oddílů nelze změnit po fakt, musíte použít některé pokročilé oddílu přístupy, jako je vytvoření nové instance služby stejného typu služby.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-170">As you cannot change the partition count after the fact, you would need to apply some advanced partition approaches, such as creating a new service instance of the same service type.</span></span> <span data-ttu-id="f0f0e-171">Musíte také implementovat logiku některé straně klienta, která směruje požadavky v instanci správné služby založené na straně klienta znalostních bází, které musí zachovat váš klientský kód.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-171">You would also need to implement some client-side logic that routes the requests to the correct service instance, based on client-side knowledge that your client code must maintain.</span></span>

<span data-ttu-id="f0f0e-172">Je potřeba vzít v úvahu pro dělení plánování prostředky počítače k dispozici.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-172">Another consideration for partitioning planning is the available computer resources.</span></span> <span data-ttu-id="f0f0e-173">Podle stavu, je třeba získat přístup a uložené, můžete je vázána na postupujte podle:</span><span class="sxs-lookup"><span data-stu-id="f0f0e-173">As the state needs to be accessed and stored, you are bound to follow:</span></span>

* <span data-ttu-id="f0f0e-174">Omezení šířky pásma sítě</span><span class="sxs-lookup"><span data-stu-id="f0f0e-174">Network bandwidth limits</span></span>
* <span data-ttu-id="f0f0e-175">Omezení paměti systému</span><span class="sxs-lookup"><span data-stu-id="f0f0e-175">System memory limits</span></span>
* <span data-ttu-id="f0f0e-176">Limity úložiště disku</span><span class="sxs-lookup"><span data-stu-id="f0f0e-176">Disk storage limits</span></span>

<span data-ttu-id="f0f0e-177">Co se tak stane, pokud k omezení prostředků v clusteru s podporou spuštěné?</span><span class="sxs-lookup"><span data-stu-id="f0f0e-177">So what happens if you run into resource constraints in a running cluster?</span></span> <span data-ttu-id="f0f0e-178">Odpověď je, že budete jednoduše škálovat cluster tak, aby dokázala pojmout nové požadavky.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-178">The answer is that you can simply scale out the cluster to accommodate the new requirements.</span></span>

<span data-ttu-id="f0f0e-179">[Příručka pro plánování kapacity](service-fabric-capacity-planning.md) nabízí pokyny, jak zjistit, kolik uzly, které cluster potřebuje.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-179">[The capacity planning guide](service-fabric-capacity-planning.md) offers guidance for how to determine how many nodes your cluster needs.</span></span>

## <a name="get-started-with-partitioning"></a><span data-ttu-id="f0f0e-180">Začínáme s dělení</span><span class="sxs-lookup"><span data-stu-id="f0f0e-180">Get started with partitioning</span></span>
<span data-ttu-id="f0f0e-181">Tato část popisuje, jak začít pracovat s vytváření oddílů služby.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-181">This section describes how to get started with partitioning your service.</span></span>

<span data-ttu-id="f0f0e-182">Service Fabric nabízí výběr ze tří schématy oddílu:</span><span class="sxs-lookup"><span data-stu-id="f0f0e-182">Service Fabric offers a choice of three partition schemes:</span></span>

* <span data-ttu-id="f0f0e-183">Pohyboval, vytváření oddílů (označováno také jako UniformInt64Partition).</span><span class="sxs-lookup"><span data-stu-id="f0f0e-183">Ranged partitioning (otherwise known as UniformInt64Partition).</span></span>
* <span data-ttu-id="f0f0e-184">S názvem rozdělení do oddílů.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-184">Named partitioning.</span></span> <span data-ttu-id="f0f0e-185">Aplikace, které používají tento model obvykle mají data, která může být bucketed, v rámci ohraničené sady.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-185">Applications using this model usually have data that can be bucketed, within a bounded set.</span></span> <span data-ttu-id="f0f0e-186">Několik běžných příkladů datová pole, které jsou použity jako klíče s názvem oddílu by oblasti, PSČ, zákazníka skupiny nebo jiné obchodní hranice.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-186">Some common examples of data fields used as named partition keys would be regions, postal codes, customer groups, or other business boundaries.</span></span>
* <span data-ttu-id="f0f0e-187">Vytváření oddílů singleton.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-187">Singleton partitioning.</span></span> <span data-ttu-id="f0f0e-188">Singleton oddíly jsou obvykle používány, když služba nevyžaduje žádné další směrování.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-188">Singleton partitions are typically used when the service does not require any additional routing.</span></span> <span data-ttu-id="f0f0e-189">Například bezstavové služby použijte toto schéma rozdělení oddílů ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-189">For example, stateless services use this partitioning scheme by default.</span></span>

<span data-ttu-id="f0f0e-190">S názvem a schémata rozdělení oddílů Singleton jsou speciální formy pohyboval oddíly.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-190">Named and Singleton partitioning schemes are special forms of ranged partitions.</span></span> <span data-ttu-id="f0f0e-191">Ve výchozím nastavení šablony sady Visual Studio pro použití Service Fabric pohyboval, vytváření oddílů, protože je jedním nejběžnějších a užitečné.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-191">By default, the Visual Studio templates for Service Fabric use ranged partitioning, as it is the most common and useful one.</span></span> <span data-ttu-id="f0f0e-192">Zbývající část tohoto článku se zaměřuje na ranged schéma rozdělení oddílů.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-192">The remainder of this article focuses on the ranged partitioning scheme.</span></span>

### <a name="ranged-partitioning-scheme"></a><span data-ttu-id="f0f0e-193">Pohyboval schéma rozdělení oddílů</span><span class="sxs-lookup"><span data-stu-id="f0f0e-193">Ranged partitioning scheme</span></span>
<span data-ttu-id="f0f0e-194">To slouží k určení rozsahu celé číslo (identifikovaný dolní klíč a vysoká hodnota klíče) a počet oddílů (ne).</span><span class="sxs-lookup"><span data-stu-id="f0f0e-194">This is used to specify an integer range (identified by a low key and high key) and a number of partitions (n).</span></span> <span data-ttu-id="f0f0e-195">Vytvoří n oddílů, každý zodpovědná za podoblast nepřekrývají sady celkového rozsahu klíče oddílu.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-195">It creates n partitions, each responsible for a non-overlapping subrange of the overall partition key range.</span></span> <span data-ttu-id="f0f0e-196">Například ranged schéma rozdělení oddílů s nízkou klíč 0, vysoká hodnota klíče 99 a počet 4 by vytvořit čtyři oddíly, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-196">For example, a ranged partitioning scheme with a low key of 0, a high key of 99, and a count of 4 would create four partitions, as shown below.</span></span>

![Vytváření oddílů v rozsahu](./media/service-fabric-concepts-partitioning/range-partitioning.png)

<span data-ttu-id="f0f0e-198">Běžný postup je vytvořit hodnotu hash na základě jedinečné klíče v datové sadě.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-198">A common approach is to create a hash based on a unique key within the data set.</span></span> <span data-ttu-id="f0f0e-199">Několik běžných příkladů klíčů by vehicle identifikační číslo (VIN), ID zaměstnance nebo do jedinečného řetězce.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-199">Some common examples of keys would be a vehicle identification number (VIN), an employee ID, or a unique string.</span></span> <span data-ttu-id="f0f0e-200">Pomocí této jedinečný klíč by pak vygeneroval hodnotu hash, numerického zbytku klíče rozsahu, chcete použít jako klíč.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-200">By using this unique key, you would then generate a hash code, modulus the key range, to use as your key.</span></span> <span data-ttu-id="f0f0e-201">Můžete zadat horní a dolní meze klíče povolený rozsah.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-201">You can specify the upper and lower bounds of the allowed key range.</span></span>

### <a name="select-a-hash-algorithm"></a><span data-ttu-id="f0f0e-202">Vyberte algoritmus hash</span><span class="sxs-lookup"><span data-stu-id="f0f0e-202">Select a hash algorithm</span></span>
<span data-ttu-id="f0f0e-203">Důležitou součástí generování hodnoty hash je výběr vaší algoritmus hash.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-203">An important part of hashing is selecting your hash algorithm.</span></span> <span data-ttu-id="f0f0e-204">Aspekt spočívá v tom, jestli je cílem chcete seskupit podobné klíče vedle sebe (polohu citlivé algoritmu hash)--nebo pokud aktivity by měl být široce distribuovány na všechny oddíly (distribuční algoritmu hash), což je dnes běžné.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-204">A consideration is whether the goal is to group similar keys near each other (locality sensitive hashing)--or if activity should be distributed broadly across all partitions (distribution hashing), which is more common.</span></span>

<span data-ttu-id="f0f0e-205">Charakteristika algoritmu hash správné distribuční jsou, je snadné výpočetní, má několik kolize a rovnoměrně distribuuje klíče.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-205">The characteristics of a good distribution hashing algorithm are that it is easy to compute, it has few collisions, and it distributes the keys evenly.</span></span> <span data-ttu-id="f0f0e-206">Dobrým příkladem algoritmu hash efektivní je [FNV-1](https://en.wikipedia.org/wiki/Fowler%E2%80%93Noll%E2%80%93Vo_hash_function) algoritmus hash.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-206">A good example of an efficient hash algorithm is the [FNV-1](https://en.wikipedia.org/wiki/Fowler%E2%80%93Noll%E2%80%93Vo_hash_function) hash algorithm.</span></span>

<span data-ttu-id="f0f0e-207">Je dobré prostředku obecné hash kód algoritmus možnosti, které [Wikipedia stránky na funkce hash](http://en.wikipedia.org/wiki/Hash_function).</span><span class="sxs-lookup"><span data-stu-id="f0f0e-207">A good resource for general hash code algorithm choices is the [Wikipedia page on hash functions](http://en.wikipedia.org/wiki/Hash_function).</span></span>

## <a name="build-a-stateful-service-with-multiple-partitions"></a><span data-ttu-id="f0f0e-208">Sestavení stavové služby s více oddílů</span><span class="sxs-lookup"><span data-stu-id="f0f0e-208">Build a stateful service with multiple partitions</span></span>
<span data-ttu-id="f0f0e-209">Umožňuje vytvořit vaše první spolehlivé stavové služby s více oddílů.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-209">Let's create your first reliable stateful service with multiple partitions.</span></span> <span data-ttu-id="f0f0e-210">V tomto příkladu vytvoříte velmi jednoduchou aplikaci, kam chcete uložit všechny poslední názvy, které začínají se stejným písmenem ve stejném oddílu.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-210">In this example, you will build a very simple application where you want to store all last names that start with the same letter in the same partition.</span></span>

<span data-ttu-id="f0f0e-211">Než napíšete kód, budete muset vezměte v úvahu oddílů a klíčů oddílů.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-211">Before you write any code, you need to think about the partitions and partition keys.</span></span> <span data-ttu-id="f0f0e-212">Potřebujete 26 oddíly, (jeden pro každou písmeno abecedy), ale co o klíčích nízkou a vysokou?</span><span class="sxs-lookup"><span data-stu-id="f0f0e-212">You need 26 partitions (one for each letter in the alphabet), but what about the low and high keys?</span></span>
<span data-ttu-id="f0f0e-213">Jako oznámena by měla obsahovat jeden oddíl na písmeno, můžeme použít 0 jako nízký klíč a 25 jako Vysoká hodnota klíče, jako je každý písmeno svůj vlastní klíč.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-213">As we literally want to have one partition per letter, we can use 0 as the low key and 25 as the high key, as each letter is its own key.</span></span>

> [!NOTE]
> <span data-ttu-id="f0f0e-214">Toto je zjednodušený scénář, ve skutečnosti by byla nevyrovnaná distribuce.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-214">This is a simplified scenario, as in reality the distribution would be uneven.</span></span> <span data-ttu-id="f0f0e-215">Příjmení začínající písmeny "S" nebo "M" jsou častější než ty, které jsou počínaje "X" nebo "Y".</span><span class="sxs-lookup"><span data-stu-id="f0f0e-215">Last names starting with the letters "S" or "M" are more common than the ones starting with "X" or "Y".</span></span>
> 
> 

1. <span data-ttu-id="f0f0e-216">Otevřete **sady Visual Studio** > **soubor** > **nové** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-216">Open **Visual Studio** > **File** > **New** > **Project**.</span></span>
2. <span data-ttu-id="f0f0e-217">V **nový projekt** dialogovém okně vyberte aplikace Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-217">In the **New Project** dialog box, choose the Service Fabric application.</span></span>
3. <span data-ttu-id="f0f0e-218">Volání projektu "AlphabetPartitions".</span><span class="sxs-lookup"><span data-stu-id="f0f0e-218">Call the project "AlphabetPartitions".</span></span>
4. <span data-ttu-id="f0f0e-219">V **vytvoření služby** dialogovém okně vyberte **Stateful** služby a pojmenujte ji "Alphabet.Processing", jak je znázorněno na obrázku níže.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-219">In the **Create a Service** dialog box, choose **Stateful** service and call it "Alphabet.Processing" as shown in the image below.</span></span>
       <span data-ttu-id="f0f0e-220">![Dialogové okno Nová služba v sadě Visual Studio][1]</span><span class="sxs-lookup"><span data-stu-id="f0f0e-220">![New service dialog in Visual Studio][1]</span></span>

  <!--  ![Stateful service screenshot](./media/service-fabric-concepts-partitioning/createstateful.png)-->

5. <span data-ttu-id="f0f0e-221">Nastavte počet oddílů.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-221">Set the number of partitions.</span></span> <span data-ttu-id="f0f0e-222">Otevřete soubor Applicationmanifest.xml umístěný ve složce ApplicationPackageRoot AlphabetPartitions projektu a aktualizovat parametr Processing_PartitionCount až 26, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-222">Open the Applicationmanifest.xml file located in the ApplicationPackageRoot folder of the AlphabetPartitions project and update the parameter Processing_PartitionCount to 26 as shown below.</span></span>
   
    ```xml
    <Parameter Name="Processing_PartitionCount" DefaultValue="26" />
    ```
   
    <span data-ttu-id="f0f0e-223">Také musíte aktualizovat vlastnosti LowKey a HighKey prvku StatefulService v ApplicationManifest.xml, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-223">You also need to update the LowKey and HighKey properties of the StatefulService element in the ApplicationManifest.xml as shown below.</span></span>
   
    ```xml
    <Service Name="Processing">
      <StatefulService ServiceTypeName="ProcessingType" TargetReplicaSetSize="[Processing_TargetReplicaSetSize]" MinReplicaSetSize="[Processing_MinReplicaSetSize]">
        <UniformInt64Partition PartitionCount="[Processing_PartitionCount]" LowKey="0" HighKey="25" />
      </StatefulService>
    </Service>
    ```
6. <span data-ttu-id="f0f0e-224">Pro službu být přístupné otevře koncový bod na portu přidáním koncový bod elementu ServiceManifest.xml (umístěný ve složce PackageRoot) pro službu Alphabet.Processing, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="f0f0e-224">For the service to be accessible, open up an endpoint on a port by adding the endpoint element of ServiceManifest.xml (located in the PackageRoot folder) for the Alphabet.Processing service as shown below:</span></span>
   
    ```xml
    <Endpoint Name="ProcessingServiceEndpoint" Port="8089" Protocol="http" Type="Internal" />
    ```
   
    <span data-ttu-id="f0f0e-225">Služba je nyní nakonfigurován pro naslouchání na vnitřní koncový bod s 26 oddíly.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-225">Now the service is configured to listen to an internal endpoint with 26 partitions.</span></span>
7. <span data-ttu-id="f0f0e-226">Dále je nutné přepsat `CreateServiceReplicaListeners()` metoda třídy zpracování.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-226">Next, you need to override the `CreateServiceReplicaListeners()` method of the Processing class.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="f0f0e-227">Tato ukázka předpokládáme, že používáte jednoduché HttpCommunicationListener.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-227">For this sample, we assume that you are using a simple HttpCommunicationListener.</span></span> <span data-ttu-id="f0f0e-228">Další informace o komunikace spolehlivé služby najdete v tématu [služba spolehlivé Service komunikační model](service-fabric-reliable-services-communication.md).</span><span class="sxs-lookup"><span data-stu-id="f0f0e-228">For more information on reliable service communication, see [The Reliable Service communication model](service-fabric-reliable-services-communication.md).</span></span>
   > 
   > 
8. <span data-ttu-id="f0f0e-229">Doporučené vzor pro repliku naslouchá na adresy URL je v následujícím formátu: `{scheme}://{nodeIp}:{port}/{partitionid}/{replicaid}/{guid}`.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-229">A recommended pattern for the URL that a replica listens on is the following format: `{scheme}://{nodeIp}:{port}/{partitionid}/{replicaid}/{guid}`.</span></span>
    <span data-ttu-id="f0f0e-230">Proto chcete provést konfiguraci vaší komunikace naslouchací proces pro naslouchání na správné koncové body a s tohoto vzoru.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-230">So you want to configure your communication listener to listen on the correct endpoints and with this pattern.</span></span>
   
    <span data-ttu-id="f0f0e-231">Víc replik této služby může být hostovaný na stejném počítači, tak tato adresa musí být jedinečný v replice.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-231">Multiple replicas of this service may be hosted on the same computer, so this address needs to be unique to the replica.</span></span> <span data-ttu-id="f0f0e-232">Z tohoto důvodu ID oddílu + ID repliky jsou v adrese URL.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-232">This is why   partition ID + replica ID are in the URL.</span></span> <span data-ttu-id="f0f0e-233">HttpListener můžete naslouchat více adres na stejném portu, dokud předponu adresy URL je jedinečný.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-233">HttpListener can listen on multiple addresses on the same port as long as the URL prefix    is unique.</span></span>
   
    <span data-ttu-id="f0f0e-234">Nadbytečné GUID je došlo k pokročilé případu, kdy sekundární repliky také naslouchání požadavkům jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-234">The extra GUID is there for an advanced case where secondary replicas also listen for read-only requests.</span></span> <span data-ttu-id="f0f0e-235">Pokud je to tento případ, chcete, abyste měli jistotu, že je novou jedinečnou adresu používat při přechodu z primárního na sekundární vynutit klienti pro překlad adres znovu.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-235">When that's the case, you want to make sure that a new unique address is used when transitioning from primary to secondary to force clients to re-resolve the address.</span></span> <span data-ttu-id="f0f0e-236">'+' se používá jako adresa zde tak, aby replika naslouchá na všech dostupných hostitelů (IP, FQDM localhost atd.) Následující kód ukazuje příklad.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-236">'+' is used as the address here so that the replica listens on all available hosts (IP, FQDM, localhost, etc.) The code below shows an example.</span></span>
   
    ```CSharp
    protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
    {
         return new[] { new ServiceReplicaListener(context => this.CreateInternalListener(context))};
    }
    private ICommunicationListener CreateInternalListener(ServiceContext context)
    {
   
         EndpointResourceDescription internalEndpoint = context.CodePackageActivationContext.GetEndpoint("ProcessingServiceEndpoint");
         string uriPrefix = String.Format(
                "{0}://+:{1}/{2}/{3}-{4}/",
                internalEndpoint.Protocol,
                internalEndpoint.Port,
                context.PartitionId,
                context.ReplicaOrInstanceId,
                Guid.NewGuid());
   
         string nodeIP = FabricRuntime.GetNodeContext().IPAddressOrFQDN;
   
         string uriPublished = uriPrefix.Replace("+", nodeIP);
         return new HttpCommunicationListener(uriPrefix, uriPublished, this.ProcessInternalRequest);
    }
    ```
   
    <span data-ttu-id="f0f0e-237">Je také vhodné poznamenat, že se mírně liší od naslouchání předponu adresy URL publikovanou adresu URL.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-237">It's also worth noting that the published URL is slightly different from the listening URL prefix.</span></span>
    <span data-ttu-id="f0f0e-238">Naslouchání adresa URL je uvedena HttpListener.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-238">The listening URL is given to HttpListener.</span></span> <span data-ttu-id="f0f0e-239">Publikovaná adresa URL je adresa URL, která je publikována ve službě Service Fabric Naming Service se používá pro zjišťování služby.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-239">The published URL is the URL that is published to the Service Fabric Naming Service, which is used for service discovery.</span></span> <span data-ttu-id="f0f0e-240">Klienti požádá pro tuto adresu prostřednictvím služby zjišťování.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-240">Clients will ask for this address through that discovery service.</span></span> <span data-ttu-id="f0f0e-241">Na adresu, kterou klienti získat musí mít skutečné IP adresu nebo plně kvalifikovaný název domény uzlu, aby bylo možné připojit.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-241">The address that clients get needs to have the actual IP or FQDN of the node in order to connect.</span></span> <span data-ttu-id="f0f0e-242">Proto je třeba nahradit '+' s uzlu IP adresu nebo plně kvalifikovaný název domény, jak je uvedeno výše.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-242">So you need to replace '+' with the node's IP or FQDN as shown above.</span></span>
9. <span data-ttu-id="f0f0e-243">Posledním krokem je přidání logika zpracování ve službě, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-243">The last step is to add the processing logic to the service as shown below.</span></span>
   
    ```CSharp
    private async Task ProcessInternalRequest(HttpListenerContext context, CancellationToken cancelRequest)
    {
        string output = null;
        string user = context.Request.QueryString["lastname"].ToString();
   
        try
        {
            output = await this.AddUserAsync(user);
        }
        catch (Exception ex)
        {
            output = ex.Message;
        }
   
        using (HttpListenerResponse response = context.Response)
        {
            if (output != null)
            {
                byte[] outBytes = Encoding.UTF8.GetBytes(output);
                response.OutputStream.Write(outBytes, 0, outBytes.Length);
            }
        }
    }
    private async Task<string> AddUserAsync(string user)
    {
        IReliableDictionary<String, String> dictionary = await this.StateManager.GetOrAddAsync<IReliableDictionary<String, String>>("dictionary");
   
        using (ITransaction tx = this.StateManager.CreateTransaction())
        {
            bool addResult = await dictionary.TryAddAsync(tx, user.ToUpperInvariant(), user);
   
            await tx.CommitAsync();
   
            return String.Format(
                "User {0} {1}",
                user,
                addResult ? "sucessfully added" : "already exists");
        }
    }
    ```
   
    <span data-ttu-id="f0f0e-244">`ProcessInternalRequest`čte hodnoty parametru řetězce dotazu, který se používá k volání oddílu a volání `AddUserAsync` přidat lastname do slovníku spolehlivé `dictionary`.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-244">`ProcessInternalRequest` reads the values of the query string parameter used to call the partition and calls `AddUserAsync` to add the lastname to the reliable dictionary `dictionary`.</span></span>
10. <span data-ttu-id="f0f0e-245">Umožňuje přidat bezstavové služby na projekt a zjistěte, jak můžete volat na konkrétní oddíl.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-245">Let's add a stateless service to the project to see how you can call a particular partition.</span></span>
    
    <span data-ttu-id="f0f0e-246">Tato služba slouží jako v jednoduchém webovém rozhraní, která přijímá lastname jako parametr řetězce dotazu, Určuje klíč oddílu a odešle ji do služby Alphabet.Processing pro zpracování.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-246">This service serves as a simple web interface that accepts the lastname as a query string parameter, determines the partition key, and sends it to the Alphabet.Processing service for processing.</span></span>
11. <span data-ttu-id="f0f0e-247">V **vytvoření služby** dialogovém okně vyberte **Stateless** služby a pojmenujte ji "Alphabet.Web", jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-247">In the **Create a Service** dialog box, choose **Stateless** service and call it "Alphabet.Web" as shown below.</span></span>
    
    ![Snímek obrazovky bezstavové služby](./media/service-fabric-concepts-partitioning/createnewstateless.png)<span data-ttu-id="f0f0e-249">.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-249">.</span></span>
12. <span data-ttu-id="f0f0e-250">Aktualizujte informace o koncový bod v ServiceManifest.xml služby Alphabet.WebApi otevře port, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-250">Update the endpoint information in the ServiceManifest.xml of the Alphabet.WebApi service to open up a port as shown below.</span></span>
    
    ```xml
    <Endpoint Name="WebApiServiceEndpoint" Protocol="http" Port="8081"/>
    ```
13. <span data-ttu-id="f0f0e-251">Budete muset vracet kolekci ServiceInstanceListeners ve třídě Web.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-251">You need to return a collection of ServiceInstanceListeners in the class Web.</span></span> <span data-ttu-id="f0f0e-252">Znovu můžete implementovat jednoduché HttpCommunicationListener.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-252">Again, you can choose to implement a simple HttpCommunicationListener.</span></span>
    
    ```CSharp
    protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
    {
        return new[] {new ServiceInstanceListener(context => this.CreateInputListener(context))};
    }
    private ICommunicationListener CreateInputListener(ServiceContext context)
    {
        // Service instance's URL is the node's IP & desired port
        EndpointResourceDescription inputEndpoint = context.CodePackageActivationContext.GetEndpoint("WebApiServiceEndpoint")
        string uriPrefix = String.Format("{0}://+:{1}/alphabetpartitions/", inputEndpoint.Protocol, inputEndpoint.Port);
        var uriPublished = uriPrefix.Replace("+", FabricRuntime.GetNodeContext().IPAddressOrFQDN);
        return new HttpCommunicationListener(uriPrefix, uriPublished, this.ProcessInputRequest);
    }
    ```
14. <span data-ttu-id="f0f0e-253">Nyní je třeba implementovat logiku zpracování.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-253">Now you need to implement the processing logic.</span></span> <span data-ttu-id="f0f0e-254">Volání HttpCommunicationListener `ProcessInputRequest` když přijde žádost.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-254">The HttpCommunicationListener calls `ProcessInputRequest` when a request comes in.</span></span> <span data-ttu-id="f0f0e-255">Proto přejdeme dopředu a přidejte následující kód.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-255">So let's go ahead and add the code below.</span></span>
    
    ```CSharp
    private async Task ProcessInputRequest(HttpListenerContext context, CancellationToken cancelRequest)
    {
        String output = null;
        try
        {
            string lastname = context.Request.QueryString["lastname"];
            char firstLetterOfLastName = lastname.First();
            ServicePartitionKey partitionKey = new ServicePartitionKey(Char.ToUpper(firstLetterOfLastName) - 'A');
    
            ResolvedServicePartition partition = await this.servicePartitionResolver.ResolveAsync(alphabetServiceUri, partitionKey, cancelRequest);
            ResolvedServiceEndpoint ep = partition.GetEndpoint();
    
            JObject addresses = JObject.Parse(ep.Address);
            string primaryReplicaAddress = (string)addresses["Endpoints"].First();
    
            UriBuilder primaryReplicaUriBuilder = new UriBuilder(primaryReplicaAddress);
            primaryReplicaUriBuilder.Query = "lastname=" + lastname;
    
            string result = await this.httpClient.GetStringAsync(primaryReplicaUriBuilder.Uri);
    
            output = String.Format(
                    "Result: {0}. <p>Partition key: '{1}' generated from the first letter '{2}' of input value '{3}'. <br>Processing service partition ID: {4}. <br>Processing service replica address: {5}",
                    result,
                    partitionKey,
                    firstLetterOfLastName,
                    lastname,
                    partition.Info.Id,
                    primaryReplicaAddress);
        }
        catch (Exception ex) { output = ex.Message; }
    
        using (var response = context.Response)
        {
            if (output != null)
            {
                output = output + "added to Partition: " + primaryReplicaAddress;
                byte[] outBytes = Encoding.UTF8.GetBytes(output);
                response.OutputStream.Write(outBytes, 0, outBytes.Length);
            }
        }
    }
    ```
    
    <span data-ttu-id="f0f0e-256">Podívejme se krok za krokem.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-256">Let's walk through it step by step.</span></span> <span data-ttu-id="f0f0e-257">Kód čte první písmeno parametru řetězce dotazu `lastname` do char.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-257">The code reads the first letter of the query string parameter `lastname` into a char.</span></span> <span data-ttu-id="f0f0e-258">Potom Určuje klíč oddílu pro toto písmeno odečtením šestnáctkové hodnoty `A` z šestnáctkové hodnoty první písmeno poslední názvy.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-258">Then, it determines the partition key for this letter by subtracting the hexadecimal value of `A` from the hexadecimal value of the last names' first letter.</span></span>
    
    ```CSharp
    string lastname = context.Request.QueryString["lastname"];
    char firstLetterOfLastName = lastname.First();
    ServicePartitionKey partitionKey = new ServicePartitionKey(Char.ToUpper(firstLetterOfLastName) - 'A');
    ```
    
    <span data-ttu-id="f0f0e-259">Pamatujte si, že v tomto příkladu že používáme 26 oddíly s klíčem jeden oddíl na oddíl.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-259">Remember, for this example, we are using 26 partitions with one partition key per partition.</span></span>
    <span data-ttu-id="f0f0e-260">V dalším kroku získáme oddílu služby `partition` pro tento klíč pomocí `ResolveAsync` metodu `servicePartitionResolver` objektu.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-260">Next, we obtain the service partition `partition` for this key by using the `ResolveAsync` method on the `servicePartitionResolver` object.</span></span> <span data-ttu-id="f0f0e-261">`servicePartitionResolver`je definován jako</span><span class="sxs-lookup"><span data-stu-id="f0f0e-261">`servicePartitionResolver` is defined as</span></span>
    
    ```CSharp
    private readonly ServicePartitionResolver servicePartitionResolver = ServicePartitionResolver.GetDefault();
    ```
    
    <span data-ttu-id="f0f0e-262">`ResolveAsync` Metoda trvá URI služby a klíč oddílu, zrušení token jako parametry.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-262">The `ResolveAsync` method takes the service URI, the partition key, and a cancellation token as parameters.</span></span> <span data-ttu-id="f0f0e-263">Služba je identifikátor URI pro službu zpracování `fabric:/AlphabetPartitions/Processing`.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-263">The service URI for the processing service is `fabric:/AlphabetPartitions/Processing`.</span></span> <span data-ttu-id="f0f0e-264">V dalším kroku se nám získat koncový bod oddílu.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-264">Next, we get the endpoint of the partition.</span></span>
    
    ```CSharp
    ResolvedServiceEndpoint ep = partition.GetEndpoint()
    ```
    
    <span data-ttu-id="f0f0e-265">Nakonec sestavit adresu URL koncového bodu plus řetězec dotazu a volání služby zpracování.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-265">Finally, we build the endpoint URL plus the querystring and call the processing service.</span></span>
    
    ```CSharp
    JObject addresses = JObject.Parse(ep.Address);
    string primaryReplicaAddress = (string)addresses["Endpoints"].First();
    
    UriBuilder primaryReplicaUriBuilder = new UriBuilder(primaryReplicaAddress);
    primaryReplicaUriBuilder.Query = "lastname=" + lastname;
    
    string result = await this.httpClient.GetStringAsync(primaryReplicaUriBuilder.Uri);
    ```
    
    <span data-ttu-id="f0f0e-266">Po dokončení zpracování výstup jsme zapsat zpět.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-266">Once the processing is done, we write the output back.</span></span>
15. <span data-ttu-id="f0f0e-267">Posledním krokem je testování služby.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-267">The last step is to test the service.</span></span> <span data-ttu-id="f0f0e-268">Parametry aplikace Visual Studio používá pro místní a cloudové nasazení.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-268">Visual Studio uses application parameters for local and cloud deployment.</span></span> <span data-ttu-id="f0f0e-269">Pokud chcete testovat službu s 26 oddíly místně, je potřeba aktualizovat `Local.xml` souborů ve složce ApplicationParameters AlphabetPartitions projektu, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="f0f0e-269">To test the service with 26 partitions locally, you need to update the `Local.xml` file in the ApplicationParameters folder of the AlphabetPartitions project as shown below:</span></span>
    
    ```xml
    <Parameters>
      <Parameter Name="Processing_PartitionCount" Value="26" />
      <Parameter Name="WebApi_InstanceCount" Value="1" />
    </Parameters>
    ```
16. <span data-ttu-id="f0f0e-270">Po dokončení nasazení můžete zkontrolovat služby a všechny její oddíly v Service Fabric Exploreru.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-270">Once you finish deployment, you can check the service and all of its partitions in the Service Fabric Explorer.</span></span>
    
    ![Snímek obrazovky Service Fabric Exploreru](./media/service-fabric-concepts-partitioning/sfxpartitions.png)
17. <span data-ttu-id="f0f0e-272">V prohlížeči, můžete otestovat Logika rozdělování zadáním `http://localhost:8081/?lastname=somename`.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-272">In a browser, you can test the partitioning logic by entering `http://localhost:8081/?lastname=somename`.</span></span> <span data-ttu-id="f0f0e-273">Zobrazí se, že každý poslední název, který začíná se stejným písmenem ukládají do stejného oddílu.</span><span class="sxs-lookup"><span data-stu-id="f0f0e-273">You will see that each last name that starts with the same letter is being stored in the same partition.</span></span>
    
    ![Snímek obrazovky prohlížeče](./media/service-fabric-concepts-partitioning/samplerunning.png)

<span data-ttu-id="f0f0e-275">Ukázky celý zdrojový kód je k dispozici na [Githubu](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/classic/Services/AlphabetPartitions).</span><span class="sxs-lookup"><span data-stu-id="f0f0e-275">The entire source code of the sample is available on [GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/classic/Services/AlphabetPartitions).</span></span>

## <a name="next-steps"></a><span data-ttu-id="f0f0e-276">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f0f0e-276">Next steps</span></span>
<span data-ttu-id="f0f0e-277">Informace o konceptech Service Fabric naleznete v následujících tématech:</span><span class="sxs-lookup"><span data-stu-id="f0f0e-277">For information on Service Fabric concepts, see the following:</span></span>

* [<span data-ttu-id="f0f0e-278">Dostupnost služeb Service Fabric</span><span class="sxs-lookup"><span data-stu-id="f0f0e-278">Availability of Service Fabric services</span></span>](service-fabric-availability-services.md)
* [<span data-ttu-id="f0f0e-279">Škálovatelnost služby Service Fabric</span><span class="sxs-lookup"><span data-stu-id="f0f0e-279">Scalability of Service Fabric services</span></span>](service-fabric-concepts-scalability.md)
* [<span data-ttu-id="f0f0e-280">Plánování kapacity pro aplikace Service Fabric</span><span class="sxs-lookup"><span data-stu-id="f0f0e-280">Capacity planning for Service Fabric applications</span></span>](service-fabric-capacity-planning.md)

[wikipartition]: https://en.wikipedia.org/wiki/Partition_(database)

[1]: ./media/service-fabric-create-your-first-application-in-visual-studio/new-project-dialog-2.png