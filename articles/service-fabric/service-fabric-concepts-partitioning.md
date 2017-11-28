---
title: "služby Service Fabric aaaPartitioning | Microsoft Docs"
description: "Popisuje, jak toopartition Service Fabric stavové služby. Oddíly umožňuje ukládání dat na místních počítačích hello tak data a výpočetní je možné rozšířit společně."
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
ms.openlocfilehash: 6ead48716c08f4212535202ee69d169067d5c6d8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="partition-service-fabric-reliable-services"></a><span data-ttu-id="6e932-104">Spolehlivé služby oddílu Service Fabric</span><span class="sxs-lookup"><span data-stu-id="6e932-104">Partition Service Fabric reliable services</span></span>
<span data-ttu-id="6e932-105">Tento článek obsahuje úvod toohello základní koncepty oddíly spolehlivé služby Azure Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="6e932-105">This article provides an introduction toohello basic concepts of partitioning Azure Service Fabric reliable services.</span></span> <span data-ttu-id="6e932-106">Hello používá se v článku hello zdrojový kód je k dispozici také na [Githubu](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/classic/Services/AlphabetPartitions).</span><span class="sxs-lookup"><span data-stu-id="6e932-106">hello source code used in hello article is also available on [GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/classic/Services/AlphabetPartitions).</span></span>

## <a name="partitioning"></a><span data-ttu-id="6e932-107">Dělení</span><span class="sxs-lookup"><span data-stu-id="6e932-107">Partitioning</span></span>
<span data-ttu-id="6e932-108">Dělení na oddíly není jedinečný tooService prostředků infrastruktury.</span><span class="sxs-lookup"><span data-stu-id="6e932-108">Partitioning is not unique tooService Fabric.</span></span> <span data-ttu-id="6e932-109">Ve skutečnosti je základní vzor vytváření škálovatelných služeb.</span><span class="sxs-lookup"><span data-stu-id="6e932-109">In fact, it is a core pattern of building scalable services.</span></span> <span data-ttu-id="6e932-110">V širším významu jsme vezměte v úvahu dělení jako koncept dělení stavu (data) a výpočetní do menší přístupné jednotky tooimprove škálovatelnost a výkon.</span><span class="sxs-lookup"><span data-stu-id="6e932-110">In a broader sense, we can think about partitioning as a concept of dividing state (data) and compute into smaller accessible units tooimprove scalability and performance.</span></span> <span data-ttu-id="6e932-111">Dobře známé formulář oddíly je [vytvoření oddílů dat][wikipartition], označované také jako horizontálního dělení.</span><span class="sxs-lookup"><span data-stu-id="6e932-111">A well-known form of partitioning is [data partitioning][wikipartition], also known as sharding.</span></span>

### <a name="partition-service-fabric-stateless-services"></a><span data-ttu-id="6e932-112">Bezstavové služby oddílu Service Fabric</span><span class="sxs-lookup"><span data-stu-id="6e932-112">Partition Service Fabric stateless services</span></span>
<span data-ttu-id="6e932-113">Pro bezstavové služby si myslíte o oddílu se logická jednotka, která obsahuje jeden nebo více instancí služby.</span><span class="sxs-lookup"><span data-stu-id="6e932-113">For stateless services, you can think about a partition being a logical unit that contains one or more instances of a service.</span></span> <span data-ttu-id="6e932-114">Obrázek 1 zobrazuje bezstavové služby s pět instancí, které jsou rozmístěny v clusteru pomocí jeden oddíl.</span><span class="sxs-lookup"><span data-stu-id="6e932-114">Figure 1 shows a stateless service with five instances distributed across a cluster using one partition.</span></span>

![Bezstavové služby](./media/service-fabric-concepts-partitioning/statelessinstances.png)

<span data-ttu-id="6e932-116">Skutečně existují dva typy řešení bezstavové služby.</span><span class="sxs-lookup"><span data-stu-id="6e932-116">There are really two types of stateless service solutions.</span></span> <span data-ttu-id="6e932-117">Hello nejdřív jednu je služba, která přetrvává stav externě, například v Azure SQL database (jako je web, který ukládá hello relace informace a data).</span><span class="sxs-lookup"><span data-stu-id="6e932-117">hello first one is a service that persists its state externally, for example in an Azure SQL database (like a website that stores hello session information and data).</span></span> <span data-ttu-id="6e932-118">Hello druhá je jen výpočetní služby (např. kalkulačky nebo bitové kopie vytváření miniatur), které nespravujete žádné trvalý stav.</span><span class="sxs-lookup"><span data-stu-id="6e932-118">hello second one is computation-only services (like a calculator or image thumbnailing) that do not manage any persistent state.</span></span>

<span data-ttu-id="6e932-119">Buď v případech dělení bezstavové služby je velmi výjimečných scénář--škálovatelnost a dostupnosti jsou obvykle dosáhnete přidáním více instancí.</span><span class="sxs-lookup"><span data-stu-id="6e932-119">In either case, partitioning a stateless service is a very rare scenario--scalability and availability are normally achieved by adding more instances.</span></span> <span data-ttu-id="6e932-120">Hello pouze, když chcete tooconsider více oddílů pro bezstavové služby instance je, když potřebujete toomeet speciální směrování požadavků.</span><span class="sxs-lookup"><span data-stu-id="6e932-120">hello only time you want tooconsider multiple partitions for stateless service instances is when you need toomeet special routing requests.</span></span>

<span data-ttu-id="6e932-121">Jako příklad Představte si případ, kdy uživatelé s identifikátory v určitý rozsah má být zpracován pouze instance konkrétní službu.</span><span class="sxs-lookup"><span data-stu-id="6e932-121">As an example, consider a case where users with IDs in a certain range should only be served by a particular service instance.</span></span> <span data-ttu-id="6e932-122">Další příklad, kdy může oddílu bezstavové služby je, když máte skutečně oddílů back-end (např. horizontálně dělené databáze SQL) a chcete, aby toocontrol měli zapsat horizontálního oddílu databáze toohello – nebo provádět jinou práci přípravy v rámci které instanci služby Hello bezstavové služby, která vyžaduje hello stejné rozdělení do oddílů informace, jak se používá v back-end hello.</span><span class="sxs-lookup"><span data-stu-id="6e932-122">Another example of when you could partition a stateless service is when you have a truly partitioned backend (e.g. a sharded SQL database) and you want toocontrol which service instance should write toohello database shard--or perform other preparation work within hello stateless service that requires hello same partitioning information as is used in hello backend.</span></span> <span data-ttu-id="6e932-123">Tyto typy scénáře lze vyřešit různými způsoby a nevyžadují nutně rozdělení do oddílů služby.</span><span class="sxs-lookup"><span data-stu-id="6e932-123">Those types of scenarios can also be solved in different ways and do not necessarily require service partitioning.</span></span>

<span data-ttu-id="6e932-124">Hello zbývající část tohoto návodu se zaměřuje na stavové služby.</span><span class="sxs-lookup"><span data-stu-id="6e932-124">hello remainder of this walkthrough focuses on stateful services.</span></span>

### <a name="partition-service-fabric-stateful-services"></a><span data-ttu-id="6e932-125">Oddíl Service Fabric stavové služby</span><span class="sxs-lookup"><span data-stu-id="6e932-125">Partition Service Fabric stateful services</span></span>
<span data-ttu-id="6e932-126">Service Fabric umožňuje snadno toodevelop škálovatelné stavové služby prostřednictvím nabídky prvotřídní způsob toopartition stavu (data).</span><span class="sxs-lookup"><span data-stu-id="6e932-126">Service Fabric makes it easy toodevelop scalable stateful services by offering a first-class way toopartition state (data).</span></span> <span data-ttu-id="6e932-127">Koncepčně, si můžete představit o oddílu stavové služby jako jednotku škálování, která je vysoce spolehlivé prostřednictvím [repliky](service-fabric-availability-services.md) které jsou distribuované a rovnoměrně rozdělen mezi hello uzlů v clusteru.</span><span class="sxs-lookup"><span data-stu-id="6e932-127">Conceptually, you can think about a partition of a stateful service as a scale unit that is highly reliable through [replicas](service-fabric-availability-services.md) that are distributed and balanced across hello nodes in a cluster.</span></span>

<span data-ttu-id="6e932-128">Vytváření oddílů v kontextu hello stavové služby Service Fabric odkazuje toohello proces pro určení toho, že oddíl konkrétní službu je zodpovědná za část hello dokončení stavu služby hello.</span><span class="sxs-lookup"><span data-stu-id="6e932-128">Partitioning in hello context of Service Fabric stateful services refers toohello process of determining that a particular service partition is responsible for a portion of hello complete state of hello service.</span></span> <span data-ttu-id="6e932-129">(Jak je uvedeno nahoře, oddíl je sada [repliky](service-fabric-availability-services.md)).</span><span class="sxs-lookup"><span data-stu-id="6e932-129">(As mentioned before, a partition is a set of [replicas](service-fabric-availability-services.md)).</span></span> <span data-ttu-id="6e932-130">Skvělé věc o Service Fabric se umístí hello oddíly v různých uzlech.</span><span class="sxs-lookup"><span data-stu-id="6e932-130">A great thing about Service Fabric is that it places hello partitions on different nodes.</span></span> <span data-ttu-id="6e932-131">To jim umožňuje limitu prostředků toogrow tooa uzlu.</span><span class="sxs-lookup"><span data-stu-id="6e932-131">This allows them toogrow tooa node's resource limit.</span></span> <span data-ttu-id="6e932-132">Jako hello data potřebuje růst, oddíly růst a Service Fabric oddíly znovu vytvoří rovnováhu mezi uzly.</span><span class="sxs-lookup"><span data-stu-id="6e932-132">As hello data needs grow, partitions grow, and Service Fabric rebalances partitions across nodes.</span></span> <span data-ttu-id="6e932-133">Tím se zajistí, že hello dál efektivní využití hardwarových prostředků.</span><span class="sxs-lookup"><span data-stu-id="6e932-133">This ensures hello continued efficient use of hardware resources.</span></span>

<span data-ttu-id="6e932-134">toogive příklad sdělení je začněte s 5 uzly clusteru a službu, která je nakonfigurovaná toohave 10 oddílů a cílem tři repliky.</span><span class="sxs-lookup"><span data-stu-id="6e932-134">toogive you an example, say you start with a 5-node cluster and a service that is configured toohave 10 partitions and a target of three replicas.</span></span> <span data-ttu-id="6e932-135">V takovém případě by vyvážit a distribuovat hello repliky do hello clusteru Service Fabric a skončili byste s dva primární [repliky](service-fabric-availability-services.md) na jeden uzel.</span><span class="sxs-lookup"><span data-stu-id="6e932-135">In this case, Service Fabric would balance and distribute hello replicas across hello cluster--and you would end up with two primary [replicas](service-fabric-availability-services.md) per node.</span></span>
<span data-ttu-id="6e932-136">Pokud potřebujete teď tooscale out hello too10 uzly, Service Fabric by znovu vyvážit hello primární [repliky](service-fabric-availability-services.md) pro všechny uzly 10.</span><span class="sxs-lookup"><span data-stu-id="6e932-136">If you now need tooscale out hello cluster too10 nodes, Service Fabric would rebalance hello primary [replicas](service-fabric-availability-services.md) across all 10 nodes.</span></span> <span data-ttu-id="6e932-137">Podobně při změně měřítka back too5 uzly Service Fabric by znovu vyvážit všechny repliky hello mezi uzly hello 5.</span><span class="sxs-lookup"><span data-stu-id="6e932-137">Likewise, if you scaled back too5 nodes, Service Fabric would rebalance all hello replicas across hello 5 nodes.</span></span>  

<span data-ttu-id="6e932-138">Obrázek 2 ukazuje hello distribuce 10 oddílů před a po škálování clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="6e932-138">Figure 2 shows hello distribution of 10 partitions before and after scaling hello cluster.</span></span>

![Stavové služby](./media/service-fabric-concepts-partitioning/partitions.png)

<span data-ttu-id="6e932-140">Hello Škálováním na více systémů se v důsledku toho dosáhne vzhledem k tomu, že požadavky od klientů, které jsou rozmístěny v počítačích, celkový výkon aplikace hello je vyšší, a tím se snižuje kolize na přístup toochunks data.</span><span class="sxs-lookup"><span data-stu-id="6e932-140">As a result, hello scale-out is achieved since requests from clients are distributed across computers, overall performance of hello application is improved, and contention on access toochunks of data is reduced.</span></span>

## <a name="plan-for-partitioning"></a><span data-ttu-id="6e932-141">Plán pro dělení</span><span class="sxs-lookup"><span data-stu-id="6e932-141">Plan for partitioning</span></span>
<span data-ttu-id="6e932-142">Před implementací služby, byste měli zvážit vždy hello dělení strategie, která je požadovaná tooscale out. Existují různé způsoby, ale všechny z nich soustředit na jaké aplikace hello musí tooachieve.</span><span class="sxs-lookup"><span data-stu-id="6e932-142">Before implementing a service, you should always consider hello partitioning strategy that is required tooscale out. There are different ways, but all of them focus on what hello application needs tooachieve.</span></span> <span data-ttu-id="6e932-143">Pro hello kontext tohoto článku zvažte některé hello důležité aspekty.</span><span class="sxs-lookup"><span data-stu-id="6e932-143">For hello context of this article, let's consider some of hello more important aspects.</span></span>

<span data-ttu-id="6e932-144">Dobrým přístupem je toothink o struktuře hello hello stavu, který potřebuje toobe rozdělena na oddíly, jako první krok hello.</span><span class="sxs-lookup"><span data-stu-id="6e932-144">A good approach is toothink about hello structure of hello state that needs toobe partitioned, as hello first step.</span></span>

<span data-ttu-id="6e932-145">Podívejme jednoduchý příklad.</span><span class="sxs-lookup"><span data-stu-id="6e932-145">Let's take a simple example.</span></span> <span data-ttu-id="6e932-146">Pokud byste byli toobuild služby pro countywide dotazování, můžete vytvořit oddíl pro každé město v okres hello.</span><span class="sxs-lookup"><span data-stu-id="6e932-146">If you were toobuild a service for a countywide poll, you could create a partition for each city in hello county.</span></span> <span data-ttu-id="6e932-147">Potom může ukládat hello hlasy pro každou osobu v hello městě hello oddílu, který odpovídá toothat města.</span><span class="sxs-lookup"><span data-stu-id="6e932-147">Then, you could store hello votes for every person in hello city in hello partition that corresponds toothat city.</span></span> <span data-ttu-id="6e932-148">Obrázek 3 znázorňuje sadu osoby a hello města, ve kterém se nacházejí.</span><span class="sxs-lookup"><span data-stu-id="6e932-148">Figure 3 illustrates a set of people and hello city in which they reside.</span></span>

![Jednoduché oddílu](./media/service-fabric-concepts-partitioning/cities.png)

<span data-ttu-id="6e932-150">Jako hello naplnění města se výrazně liší, můžete skončit s některé oddíly, které obsahují velké množství dat (např. Praha) a ostatní oddíly s velmi malé stavu (např. Kirkland).</span><span class="sxs-lookup"><span data-stu-id="6e932-150">As hello population of cities varies widely, you may end up with some partitions that contain a lot of data (e.g. Seattle) and other partitions with very little state (e.g. Kirkland).</span></span> <span data-ttu-id="6e932-151">A co je hello dopad s oddíly s nerovnoměrné objemy stavu?</span><span class="sxs-lookup"><span data-stu-id="6e932-151">So what is hello impact of having partitions with uneven amounts of state?</span></span>

<span data-ttu-id="6e932-152">Pokud přemýšlíte o příklad hello znovu, snadno uvidíte, že hello oddílu, který obsahuje, zda text hello hlasy pro Seattle získáte více požadavků než hello Kirkland jeden.</span><span class="sxs-lookup"><span data-stu-id="6e932-152">If you think about hello example again, you can easily see that hello partition that holds hello votes for Seattle will get more traffic than hello Kirkland one.</span></span> <span data-ttu-id="6e932-153">Ve výchozím nastavení, Service Fabric zajišťuje, že je o hello stejný počet primární a sekundární repliky na každém uzlu.</span><span class="sxs-lookup"><span data-stu-id="6e932-153">By default, Service Fabric makes sure that there is about hello same number of primary and secondary replicas on each node.</span></span> <span data-ttu-id="6e932-154">Proto můžete skončit s uzly, které mají replik, které poskytovat další provoz a ostatní, které slouží menší provoz.</span><span class="sxs-lookup"><span data-stu-id="6e932-154">So you may end up with nodes that hold replicas that serve more traffic and others that serve less traffic.</span></span> <span data-ttu-id="6e932-155">By pokud možno chcete tooavoid aktivní a cold body, jako to v clusteru.</span><span class="sxs-lookup"><span data-stu-id="6e932-155">You would preferably want tooavoid hot and cold spots like this in a cluster.</span></span>

<span data-ttu-id="6e932-156">V pořadí tooavoid, to, měli byste udělat dvě věci, z hlediska rozdělení oddílů:</span><span class="sxs-lookup"><span data-stu-id="6e932-156">In order tooavoid this, you should do two things, from a partitioning point of view:</span></span>

* <span data-ttu-id="6e932-157">Zkuste toopartition hello stavu tak, aby je rovnoměrně rozdělené mezi všechny oddíly.</span><span class="sxs-lookup"><span data-stu-id="6e932-157">Try toopartition hello state so that it is evenly distributed across all partitions.</span></span>
* <span data-ttu-id="6e932-158">Načtení sestav z každou repliku hello služby hello.</span><span class="sxs-lookup"><span data-stu-id="6e932-158">Report load from each of hello replicas for hello service.</span></span> <span data-ttu-id="6e932-159">(Informace o tom, projděte si tento článek na [metriky a zatížení](service-fabric-cluster-resource-manager-metrics.md)).</span><span class="sxs-lookup"><span data-stu-id="6e932-159">(For information on how, check out this article on [Metrics and Load](service-fabric-cluster-resource-manager-metrics.md)).</span></span> <span data-ttu-id="6e932-160">Service Fabric nabízí hello schopností tooreport zatížení spotřebovávají služby, jako je množství paměti nebo počet záznamů.</span><span class="sxs-lookup"><span data-stu-id="6e932-160">Service Fabric provides hello capability tooreport load consumed by services, such as amount of memory or number of records.</span></span> <span data-ttu-id="6e932-161">Na základě hello metriky hlášené, Service Fabric zjistí, že některé oddíly slouží větší objemy než jiné a znovu vytvoří rovnováhu hello clusteru přesunutí repliky toomore vhodný uzly, tak, aby celkový je přetížena žádný uzel.</span><span class="sxs-lookup"><span data-stu-id="6e932-161">Based on hello metrics reported, Service Fabric detects that some partitions are serving higher loads than others and rebalances hello cluster by moving replicas toomore suitable nodes, so that overall no node is overloaded.</span></span>

<span data-ttu-id="6e932-162">V některých případech nelze vědět, kolik dat bude v daném oddílu.</span><span class="sxs-lookup"><span data-stu-id="6e932-162">Sometimes, you cannot know how much data will be in a given partition.</span></span> <span data-ttu-id="6e932-163">Obecné doporučení je toodo obou – nejprve přijetím strategie dělení který se šíří hello data rovnoměrně mezi oddílů hello a druhý, pomocí sestav zatížení.</span><span class="sxs-lookup"><span data-stu-id="6e932-163">So a general recommendation is toodo both--first, by adopting a partitioning strategy that spreads hello data evenly across hello partitions and second, by reporting load.</span></span>  <span data-ttu-id="6e932-164">První metoda Hello zabraňuje případů popsaných v hello hlasování příklad, zatímco hello druhý pomáhá vyhlazení dočasné rozdíly v přístupu nebo zatížení v čase.</span><span class="sxs-lookup"><span data-stu-id="6e932-164">hello first method prevents situations described in hello voting example, while hello second helps smooth out temporary differences in access or load over time.</span></span>

<span data-ttu-id="6e932-165">Další aspekt plánování oddílu je toochoose hello správný počet oddílů toobegin s.</span><span class="sxs-lookup"><span data-stu-id="6e932-165">Another aspect of partition planning is toochoose hello correct number of partitions toobegin with.</span></span>
<span data-ttu-id="6e932-166">Z hlediska Service Fabric není nic, které zabraňují začínáte s vyšší počet oddílů, než se očekává pro váš scénář.</span><span class="sxs-lookup"><span data-stu-id="6e932-166">From a Service Fabric perspective, there is nothing that prevents you from starting out with a higher number of partitions than anticipated for your scenario.</span></span>
<span data-ttu-id="6e932-167">Za předpokladu, že hello maximální počet oddílů ve skutečnosti je platný přístup.</span><span class="sxs-lookup"><span data-stu-id="6e932-167">In fact, assuming hello maximum number of partitions is a valid approach.</span></span>

<span data-ttu-id="6e932-168">Ve výjimečných případech můžete dospět nutnosti více oddílů, než jste původně vybrali.</span><span class="sxs-lookup"><span data-stu-id="6e932-168">In rare cases, you may end up needing more partitions than you have initially chosen.</span></span> <span data-ttu-id="6e932-169">Jako počet oddílů hello nelze změnit po hello fakt, potřebovali byste tooapply přístupy některé pokročilé oddílu, jako je vytvoření nové instance služby hello stejný typ služby.</span><span class="sxs-lookup"><span data-stu-id="6e932-169">As you cannot change hello partition count after hello fact, you would need tooapply some advanced partition approaches, such as creating a new service instance of hello same service type.</span></span> <span data-ttu-id="6e932-170">Také potřebujete tooimplement některé klientské logiky, která směruje hello požadavky instance správné služby toohello, založené na straně klienta znalostních bází, které musí zachovat váš klientský kód.</span><span class="sxs-lookup"><span data-stu-id="6e932-170">You would also need tooimplement some client-side logic that routes hello requests toohello correct service instance, based on client-side knowledge that your client code must maintain.</span></span>

<span data-ttu-id="6e932-171">Je potřeba vzít v úvahu pro dělení plánování hello dostupný počítač prostředky.</span><span class="sxs-lookup"><span data-stu-id="6e932-171">Another consideration for partitioning planning is hello available computer resources.</span></span> <span data-ttu-id="6e932-172">Stav hello stačit toobe přístup a uložené, jste vázané toofollow:</span><span class="sxs-lookup"><span data-stu-id="6e932-172">As hello state needs toobe accessed and stored, you are bound toofollow:</span></span>

* <span data-ttu-id="6e932-173">Omezení šířky pásma sítě</span><span class="sxs-lookup"><span data-stu-id="6e932-173">Network bandwidth limits</span></span>
* <span data-ttu-id="6e932-174">Omezení paměti systému</span><span class="sxs-lookup"><span data-stu-id="6e932-174">System memory limits</span></span>
* <span data-ttu-id="6e932-175">Limity úložiště disku</span><span class="sxs-lookup"><span data-stu-id="6e932-175">Disk storage limits</span></span>

<span data-ttu-id="6e932-176">Co se tak stane, pokud k omezení prostředků v clusteru s podporou spuštěné? odpověď Hello je, že budete jednoduše škálovat hello clusteru tooaccommodate hello nové požadavky.</span><span class="sxs-lookup"><span data-stu-id="6e932-176">So what happens if you run into resource constraints in a running cluster? hello answer is that you can simply scale out hello cluster tooaccommodate hello new requirements.</span></span>

<span data-ttu-id="6e932-177">[Příručka pro plánování kapacity Hello](service-fabric-capacity-planning.md) nabízí pokyny, jak toodetermine kolik uzly, které cluster potřebuje.</span><span class="sxs-lookup"><span data-stu-id="6e932-177">[hello capacity planning guide](service-fabric-capacity-planning.md) offers guidance for how toodetermine how many nodes your cluster needs.</span></span>

## <a name="get-started-with-partitioning"></a><span data-ttu-id="6e932-178">Začínáme s dělení</span><span class="sxs-lookup"><span data-stu-id="6e932-178">Get started with partitioning</span></span>
<span data-ttu-id="6e932-179">Tato část popisuje, jak se tooget pracovat s vytváření oddílů služby.</span><span class="sxs-lookup"><span data-stu-id="6e932-179">This section describes how tooget started with partitioning your service.</span></span>

<span data-ttu-id="6e932-180">Service Fabric nabízí výběr ze tří schématy oddílu:</span><span class="sxs-lookup"><span data-stu-id="6e932-180">Service Fabric offers a choice of three partition schemes:</span></span>

* <span data-ttu-id="6e932-181">Pohyboval, vytváření oddílů (označováno také jako UniformInt64Partition).</span><span class="sxs-lookup"><span data-stu-id="6e932-181">Ranged partitioning (otherwise known as UniformInt64Partition).</span></span>
* <span data-ttu-id="6e932-182">S názvem rozdělení do oddílů.</span><span class="sxs-lookup"><span data-stu-id="6e932-182">Named partitioning.</span></span> <span data-ttu-id="6e932-183">Aplikace, které používají tento model obvykle mají data, která může být bucketed, v rámci ohraničené sady.</span><span class="sxs-lookup"><span data-stu-id="6e932-183">Applications using this model usually have data that can be bucketed, within a bounded set.</span></span> <span data-ttu-id="6e932-184">Několik běžných příkladů datová pole, které jsou použity jako klíče s názvem oddílu by oblasti, PSČ, zákazníka skupiny nebo jiné obchodní hranice.</span><span class="sxs-lookup"><span data-stu-id="6e932-184">Some common examples of data fields used as named partition keys would be regions, postal codes, customer groups, or other business boundaries.</span></span>
* <span data-ttu-id="6e932-185">Vytváření oddílů singleton.</span><span class="sxs-lookup"><span data-stu-id="6e932-185">Singleton partitioning.</span></span> <span data-ttu-id="6e932-186">Singleton oddíly jsou obvykle používány, když služba hello nevyžaduje žádné další směrování.</span><span class="sxs-lookup"><span data-stu-id="6e932-186">Singleton partitions are typically used when hello service does not require any additional routing.</span></span> <span data-ttu-id="6e932-187">Například bezstavové služby použijte toto schéma rozdělení oddílů ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="6e932-187">For example, stateless services use this partitioning scheme by default.</span></span>

<span data-ttu-id="6e932-188">S názvem a schémata rozdělení oddílů Singleton jsou speciální formy pohyboval oddíly.</span><span class="sxs-lookup"><span data-stu-id="6e932-188">Named and Singleton partitioning schemes are special forms of ranged partitions.</span></span> <span data-ttu-id="6e932-189">Ve výchozím nastavení hello šablony sady Visual Studio pro použití Service Fabric pohyboval oddílů, protože je hello jedním nejběžnějších a užitečné.</span><span class="sxs-lookup"><span data-stu-id="6e932-189">By default, hello Visual Studio templates for Service Fabric use ranged partitioning, as it is hello most common and useful one.</span></span> <span data-ttu-id="6e932-190">Hello zbývající část tohoto článku se zaměřuje na schéma rozdělení oddílů pohyboval hello.</span><span class="sxs-lookup"><span data-stu-id="6e932-190">hello remainder of this article focuses on hello ranged partitioning scheme.</span></span>

### <a name="ranged-partitioning-scheme"></a><span data-ttu-id="6e932-191">Pohyboval schéma rozdělení oddílů</span><span class="sxs-lookup"><span data-stu-id="6e932-191">Ranged partitioning scheme</span></span>
<span data-ttu-id="6e932-192">Toto je použité toospecify celé číslo v rozsahu (identifikovaný dolní klíč a vysoká hodnota klíče) a počet oddílů (ne).</span><span class="sxs-lookup"><span data-stu-id="6e932-192">This is used toospecify an integer range (identified by a low key and high key) and a number of partitions (n).</span></span> <span data-ttu-id="6e932-193">Vytvoří n oddílů, každý zodpovědná za Podoblast sady nepřekrývají hello celkový rozsah klíče oddílu.</span><span class="sxs-lookup"><span data-stu-id="6e932-193">It creates n partitions, each responsible for a non-overlapping subrange of hello overall partition key range.</span></span> <span data-ttu-id="6e932-194">Například ranged schéma rozdělení oddílů s nízkou klíč 0, vysoká hodnota klíče 99 a počet 4 by vytvořit čtyři oddíly, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="6e932-194">For example, a ranged partitioning scheme with a low key of 0, a high key of 99, and a count of 4 would create four partitions, as shown below.</span></span>

![Vytváření oddílů v rozsahu](./media/service-fabric-concepts-partitioning/range-partitioning.png)

<span data-ttu-id="6e932-196">Běžný postup je toocreate hodnotu hash na základě jedinečné klíče v rámci sady dat hello.</span><span class="sxs-lookup"><span data-stu-id="6e932-196">A common approach is toocreate a hash based on a unique key within hello data set.</span></span> <span data-ttu-id="6e932-197">Několik běžných příkladů klíčů by vehicle identifikační číslo (VIN), ID zaměstnance nebo do jedinečného řetězce.</span><span class="sxs-lookup"><span data-stu-id="6e932-197">Some common examples of keys would be a vehicle identification number (VIN), an employee ID, or a unique string.</span></span> <span data-ttu-id="6e932-198">Pomocí této jedinečný klíč by pak generovat kód hash, numerického zbytku hello klíče rozsahu, toouse jako klíč.</span><span class="sxs-lookup"><span data-stu-id="6e932-198">By using this unique key, you would then generate a hash code, modulus hello key range, toouse as your key.</span></span> <span data-ttu-id="6e932-199">Můžete zadat hello horní a dolní meze hello povolený rozsah klíče.</span><span class="sxs-lookup"><span data-stu-id="6e932-199">You can specify hello upper and lower bounds of hello allowed key range.</span></span>

### <a name="select-a-hash-algorithm"></a><span data-ttu-id="6e932-200">Vyberte algoritmus hash</span><span class="sxs-lookup"><span data-stu-id="6e932-200">Select a hash algorithm</span></span>
<span data-ttu-id="6e932-201">Důležitou součástí generování hodnoty hash je výběr vaší algoritmus hash.</span><span class="sxs-lookup"><span data-stu-id="6e932-201">An important part of hashing is selecting your hash algorithm.</span></span> <span data-ttu-id="6e932-202">Aspekt spočívá v tom, zda text hello cílem je toogroup podobné klíče vedle sebe (polohu citlivé algoritmu hash)--nebo pokud aktivity by měl být široce distribuovány na všechny oddíly (distribuční algoritmu hash), což je dnes běžné.</span><span class="sxs-lookup"><span data-stu-id="6e932-202">A consideration is whether hello goal is toogroup similar keys near each other (locality sensitive hashing)--or if activity should be distributed broadly across all partitions (distribution hashing), which is more common.</span></span>

<span data-ttu-id="6e932-203">Hello vlastnosti algoritmu hash správné distribuční jsou je snadno toocompute, má několik kolize a rovnoměrně distribuuje hello klíče.</span><span class="sxs-lookup"><span data-stu-id="6e932-203">hello characteristics of a good distribution hashing algorithm are that it is easy toocompute, it has few collisions, and it distributes hello keys evenly.</span></span> <span data-ttu-id="6e932-204">Dobrým příkladem algoritmu hash efektivní je hello [FNV-1](https://en.wikipedia.org/wiki/Fowler%E2%80%93Noll%E2%80%93Vo_hash_function) algoritmus hash.</span><span class="sxs-lookup"><span data-stu-id="6e932-204">A good example of an efficient hash algorithm is hello [FNV-1](https://en.wikipedia.org/wiki/Fowler%E2%80%93Noll%E2%80%93Vo_hash_function) hash algorithm.</span></span>

<span data-ttu-id="6e932-205">Dobrý prostředku obecné hash kód algoritmus možnosti, které je hello [Wikipedia stránky na funkce hash](http://en.wikipedia.org/wiki/Hash_function).</span><span class="sxs-lookup"><span data-stu-id="6e932-205">A good resource for general hash code algorithm choices is hello [Wikipedia page on hash functions](http://en.wikipedia.org/wiki/Hash_function).</span></span>

## <a name="build-a-stateful-service-with-multiple-partitions"></a><span data-ttu-id="6e932-206">Sestavení stavové služby s více oddílů</span><span class="sxs-lookup"><span data-stu-id="6e932-206">Build a stateful service with multiple partitions</span></span>
<span data-ttu-id="6e932-207">Umožňuje vytvořit vaše první spolehlivé stavové služby s více oddílů.</span><span class="sxs-lookup"><span data-stu-id="6e932-207">Let's create your first reliable stateful service with multiple partitions.</span></span> <span data-ttu-id="6e932-208">V tomto příkladu vytvoříte velmi jednoduchou aplikaci, kam chcete toostore všechny poslední názvy, které začínají hello stejné písmeno v hello stejného oddílu.</span><span class="sxs-lookup"><span data-stu-id="6e932-208">In this example, you will build a very simple application where you want toostore all last names that start with hello same letter in hello same partition.</span></span>

<span data-ttu-id="6e932-209">Než napíšete kód, je nutné toothink o hello oddílů a klíčů oddílů.</span><span class="sxs-lookup"><span data-stu-id="6e932-209">Before you write any code, you need toothink about hello partitions and partition keys.</span></span> <span data-ttu-id="6e932-210">Potřebujete 26 oddíly (jeden pro každou písmeno v hello abecedy), ale co o hello nízkou a vysokou klíčů?</span><span class="sxs-lookup"><span data-stu-id="6e932-210">You need 26 partitions (one for each letter in hello alphabet), but what about hello low and high keys?</span></span>
<span data-ttu-id="6e932-211">Jak chceme oznámena toohave jeden oddíl na písmeno, můžeme použít 0 jako hello dolní klíč a 25 jako hello vysoká hodnota klíče, jako je každý písmeno svůj vlastní klíč.</span><span class="sxs-lookup"><span data-stu-id="6e932-211">As we literally want toohave one partition per letter, we can use 0 as hello low key and 25 as hello high key, as each letter is its own key.</span></span>

> [!NOTE]
> <span data-ttu-id="6e932-212">Toto je zjednodušený scénář, ve skutečnosti by byla nevyrovnaná distribuce hello.</span><span class="sxs-lookup"><span data-stu-id="6e932-212">This is a simplified scenario, as in reality hello distribution would be uneven.</span></span> <span data-ttu-id="6e932-213">Příjmení začíná hello písmena "S" nebo "M" jsou častější než ty, které jsou hello začíná "X" nebo "Y".</span><span class="sxs-lookup"><span data-stu-id="6e932-213">Last names starting with hello letters "S" or "M" are more common than hello ones starting with "X" or "Y".</span></span>
> 
> 

1. <span data-ttu-id="6e932-214">Otevřete **sady Visual Studio** > **soubor** > **nové** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="6e932-214">Open **Visual Studio** > **File** > **New** > **Project**.</span></span>
2. <span data-ttu-id="6e932-215">V hello **nový projekt** dialogovém okně vyberte aplikace Service Fabric hello.</span><span class="sxs-lookup"><span data-stu-id="6e932-215">In hello **New Project** dialog box, choose hello Service Fabric application.</span></span>
3. <span data-ttu-id="6e932-216">Volání projektu hello "AlphabetPartitions".</span><span class="sxs-lookup"><span data-stu-id="6e932-216">Call hello project "AlphabetPartitions".</span></span>
4. <span data-ttu-id="6e932-217">V hello **vytvoření služby** dialogovém okně vyberte **Stateful** služby a pojmenujte ji "Alphabet.Processing", jak ukazuje následující obrázek hello.</span><span class="sxs-lookup"><span data-stu-id="6e932-217">In hello **Create a Service** dialog box, choose **Stateful** service and call it "Alphabet.Processing" as shown in hello image below.</span></span>
       <span data-ttu-id="6e932-218">![Dialogové okno Nová služba v sadě Visual Studio][1]</span><span class="sxs-lookup"><span data-stu-id="6e932-218">![New service dialog in Visual Studio][1]</span></span>

  <!--  ![Stateful service screenshot](./media/service-fabric-concepts-partitioning/createstateful.png)-->

5. <span data-ttu-id="6e932-219">Nastavit hello počet oddílů.</span><span class="sxs-lookup"><span data-stu-id="6e932-219">Set hello number of partitions.</span></span> <span data-ttu-id="6e932-220">Otevřete hello Applicationmanifest.xml soubor umístěný ve hello ApplicationPackageRoot složky hello AlphabetPartitions projekt a aktualizace hello parametru Processing_PartitionCount too26, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="6e932-220">Open hello Applicationmanifest.xml file located in hello ApplicationPackageRoot folder of hello AlphabetPartitions project and update hello parameter Processing_PartitionCount too26 as shown below.</span></span>
   
    ```xml
    <Parameter Name="Processing_PartitionCount" DefaultValue="26" />
    ```
   
    <span data-ttu-id="6e932-221">Musíte taky tooupdate hello LowKey a HighKey vlastnosti elementu StatefulService hello hello ApplicationManifest.xml, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="6e932-221">You also need tooupdate hello LowKey and HighKey properties of hello StatefulService element in hello ApplicationManifest.xml as shown below.</span></span>
   
    ```xml
    <Service Name="Processing">
      <StatefulService ServiceTypeName="ProcessingType" TargetReplicaSetSize="[Processing_TargetReplicaSetSize]" MinReplicaSetSize="[Processing_MinReplicaSetSize]">
        <UniformInt64Partition PartitionCount="[Processing_PartitionCount]" LowKey="0" HighKey="25" />
      </StatefulService>
    </Service>
    ```
6. <span data-ttu-id="6e932-222">Pro služby toobe hello přístupný otevře koncový bod na portu přidáním hello koncový bod elementu ServiceManifest.xml (umístěný ve složce PackageRoot hello) pro hello Alphabet.Processing služby, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="6e932-222">For hello service toobe accessible, open up an endpoint on a port by adding hello endpoint element of ServiceManifest.xml (located in hello PackageRoot folder) for hello Alphabet.Processing service as shown below:</span></span>
   
    ```xml
    <Endpoint Name="ProcessingServiceEndpoint" Port="8089" Protocol="http" Type="Internal" />
    ```
   
    <span data-ttu-id="6e932-223">Nyní je služba hello nakonfigurované toolisten tooan vnitřní koncový bod s 26 oddíly.</span><span class="sxs-lookup"><span data-stu-id="6e932-223">Now hello service is configured toolisten tooan internal endpoint with 26 partitions.</span></span>
7. <span data-ttu-id="6e932-224">Dále musíte toooverride hello `CreateServiceReplicaListeners()` metoda hello zpracování třídy.</span><span class="sxs-lookup"><span data-stu-id="6e932-224">Next, you need toooverride hello `CreateServiceReplicaListeners()` method of hello Processing class.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="6e932-225">Tato ukázka předpokládáme, že používáte jednoduché HttpCommunicationListener.</span><span class="sxs-lookup"><span data-stu-id="6e932-225">For this sample, we assume that you are using a simple HttpCommunicationListener.</span></span> <span data-ttu-id="6e932-226">Další informace o komunikace spolehlivé služby najdete v tématu [hello spolehlivá služba komunikační model](service-fabric-reliable-services-communication.md).</span><span class="sxs-lookup"><span data-stu-id="6e932-226">For more information on reliable service communication, see [hello Reliable Service communication model](service-fabric-reliable-services-communication.md).</span></span>
   > 
   > 
8. <span data-ttu-id="6e932-227">Doporučené vzor hello adresu URL, která replika naslouchá na je hello následující formát: `{scheme}://{nodeIp}:{port}/{partitionid}/{replicaid}/{guid}`.</span><span class="sxs-lookup"><span data-stu-id="6e932-227">A recommended pattern for hello URL that a replica listens on is hello following format: `{scheme}://{nodeIp}:{port}/{partitionid}/{replicaid}/{guid}`.</span></span>
    <span data-ttu-id="6e932-228">Proto je vhodné tooconfigure vaší komunikace toolisten naslouchací proces na správné koncové body hello a s tohoto vzoru.</span><span class="sxs-lookup"><span data-stu-id="6e932-228">So you want tooconfigure your communication listener toolisten on hello correct endpoints and with this pattern.</span></span>
   
    <span data-ttu-id="6e932-229">Víc replik tato služba může být hostitelem hello stejný počítač, takže tato adresa musí toobe jedinečný toohello repliky.</span><span class="sxs-lookup"><span data-stu-id="6e932-229">Multiple replicas of this service may be hosted on hello same computer, so this address needs toobe unique toohello replica.</span></span> <span data-ttu-id="6e932-230">Z tohoto důvodu ID oddílu + ID repliky jsou v adrese URL hello.</span><span class="sxs-lookup"><span data-stu-id="6e932-230">This is why   partition ID + replica ID are in hello URL.</span></span> <span data-ttu-id="6e932-231">HttpListener můžete naslouchat na více adres na stejný port, dokud je jedinečnou předponu adresy URL hello hello.</span><span class="sxs-lookup"><span data-stu-id="6e932-231">HttpListener can listen on multiple addresses on hello same port as long as hello URL prefix    is unique.</span></span>
   
    <span data-ttu-id="6e932-232">Dobrý den, který je navíc GUID k pokročilé případu, kdy sekundární repliky také naslouchání požadavkům jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="6e932-232">hello extra GUID is there for an advanced case where secondary replicas also listen for read-only requests.</span></span> <span data-ttu-id="6e932-233">Po hello případ, budete chtít toomake jistotu, že se při přechodu z primární toosecondary tooforce klienti toore vyřešit hello adresy slouží novou jedinečnou adresu.</span><span class="sxs-lookup"><span data-stu-id="6e932-233">When that's hello case, you want toomake sure that a new unique address is used when transitioning from primary toosecondary tooforce clients toore-resolve hello address.</span></span> <span data-ttu-id="6e932-234">'+' se používá jako hello adresy tady tak, aby hello replika naslouchá na všech dostupných hostitelů (IP, FQDM, localhost atd.) hello následující kód ukazuje příklad.</span><span class="sxs-lookup"><span data-stu-id="6e932-234">'+' is used as hello address here so that hello replica listens on all available hosts (IP, FQDM, localhost, etc.) hello code below shows an example.</span></span>
   
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
   
    <span data-ttu-id="6e932-235">Je také vhodné poznamenat, že hello publikovat adresu URL se mírně liší od hello naslouchání předponu adresy URL.</span><span class="sxs-lookup"><span data-stu-id="6e932-235">It's also worth noting that hello published URL is slightly different from hello listening URL prefix.</span></span>
    <span data-ttu-id="6e932-236">Adresa URL naslouchání Hello dostane tooHttpListener.</span><span class="sxs-lookup"><span data-stu-id="6e932-236">hello listening URL is given tooHttpListener.</span></span> <span data-ttu-id="6e932-237">Hello hello adresu URL, která je publikovaná toohello služby prostředků infrastruktury služby DNS, který se používá pro zjišťování služby je publikovaná adresa URL.</span><span class="sxs-lookup"><span data-stu-id="6e932-237">hello published URL is hello URL that is published toohello Service Fabric Naming Service, which is used for service discovery.</span></span> <span data-ttu-id="6e932-238">Klienti požádá pro tuto adresu prostřednictvím služby zjišťování.</span><span class="sxs-lookup"><span data-stu-id="6e932-238">Clients will ask for this address through that discovery service.</span></span> <span data-ttu-id="6e932-239">Adresa Hello klientům získat potřebám toohave hello skutečné IP adresu nebo plně kvalifikovaný název domény uzlu hello v tooconnect pořadí.</span><span class="sxs-lookup"><span data-stu-id="6e932-239">hello address that clients get needs toohave hello actual IP or FQDN of hello node in order tooconnect.</span></span> <span data-ttu-id="6e932-240">Proto musíte tooreplace '+' s hello uzlu IP adresu nebo plně kvalifikovaný název domény jako uvedené výše.</span><span class="sxs-lookup"><span data-stu-id="6e932-240">So you need tooreplace '+' with hello node's IP or FQDN as shown above.</span></span>
9. <span data-ttu-id="6e932-241">posledním krokem Hello je tooadd hello zpracování logiky toohello služby, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="6e932-241">hello last step is tooadd hello processing logic toohello service as shown below.</span></span>
   
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
   
    <span data-ttu-id="6e932-242">`ProcessInternalRequest`čtení hello hodnoty hello dotazu řetězec parametr použitý toocall hello oddílu a volání `AddUserAsync` tooadd hello lastname toohello spolehlivé slovník `dictionary`.</span><span class="sxs-lookup"><span data-stu-id="6e932-242">`ProcessInternalRequest` reads hello values of hello query string parameter used toocall hello partition and calls `AddUserAsync` tooadd hello lastname toohello reliable dictionary `dictionary`.</span></span>
10. <span data-ttu-id="6e932-243">Umožňuje přidat bezstavové služby toohello projektu toosee jak můžete volat na konkrétní oddíl.</span><span class="sxs-lookup"><span data-stu-id="6e932-243">Let's add a stateless service toohello project toosee how you can call a particular partition.</span></span>
    
    <span data-ttu-id="6e932-244">Tato služba slouží jako v jednoduchém webovém rozhraní, která přijímá hello lastname jako parametr řetězce dotazu, Určuje klíč oddílu hello a odešle ji toohello Alphabet.Processing služby pro zpracování.</span><span class="sxs-lookup"><span data-stu-id="6e932-244">This service serves as a simple web interface that accepts hello lastname as a query string parameter, determines hello partition key, and sends it toohello Alphabet.Processing service for processing.</span></span>
11. <span data-ttu-id="6e932-245">V hello **vytvoření služby** dialogovém okně vyberte **Stateless** služby a pojmenujte ji "Alphabet.Web", jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="6e932-245">In hello **Create a Service** dialog box, choose **Stateless** service and call it "Alphabet.Web" as shown below.</span></span>
    
    ![Snímek obrazovky bezstavové služby](./media/service-fabric-concepts-partitioning/createnewstateless.png)<span data-ttu-id="6e932-247">.</span><span class="sxs-lookup"><span data-stu-id="6e932-247">.</span></span>
12. <span data-ttu-id="6e932-248">Aktualizujte informace hello koncového bodu v hello ServiceManifest.xml hello Alphabet.WebApi služby tooopen až portu, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="6e932-248">Update hello endpoint information in hello ServiceManifest.xml of hello Alphabet.WebApi service tooopen up a port as shown below.</span></span>
    
    ```xml
    <Endpoint Name="WebApiServiceEndpoint" Protocol="http" Port="8081"/>
    ```
13. <span data-ttu-id="6e932-249">Je nutné tooreturn kolekce ServiceInstanceListeners ve třídě hello Web.</span><span class="sxs-lookup"><span data-stu-id="6e932-249">You need tooreturn a collection of ServiceInstanceListeners in hello class Web.</span></span> <span data-ttu-id="6e932-250">Znovu můžete zvolit tooimplement jednoduché HttpCommunicationListener.</span><span class="sxs-lookup"><span data-stu-id="6e932-250">Again, you can choose tooimplement a simple HttpCommunicationListener.</span></span>
    
    ```CSharp
    protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
    {
        return new[] {new ServiceInstanceListener(context => this.CreateInputListener(context))};
    }
    private ICommunicationListener CreateInputListener(ServiceContext context)
    {
        // Service instance's URL is hello node's IP & desired port
        EndpointResourceDescription inputEndpoint = context.CodePackageActivationContext.GetEndpoint("WebApiServiceEndpoint")
        string uriPrefix = String.Format("{0}://+:{1}/alphabetpartitions/", inputEndpoint.Protocol, inputEndpoint.Port);
        var uriPublished = uriPrefix.Replace("+", FabricRuntime.GetNodeContext().IPAddressOrFQDN);
        return new HttpCommunicationListener(uriPrefix, uriPublished, this.ProcessInputRequest);
    }
    ```
14. <span data-ttu-id="6e932-251">Teď musíte tooimplement hello zpracování logiky.</span><span class="sxs-lookup"><span data-stu-id="6e932-251">Now you need tooimplement hello processing logic.</span></span> <span data-ttu-id="6e932-252">Hello HttpCommunicationListener volání `ProcessInputRequest` když přijde žádost.</span><span class="sxs-lookup"><span data-stu-id="6e932-252">hello HttpCommunicationListener calls `ProcessInputRequest` when a request comes in.</span></span> <span data-ttu-id="6e932-253">Proto přejdeme dopředu a přidejte následující kód hello.</span><span class="sxs-lookup"><span data-stu-id="6e932-253">So let's go ahead and add hello code below.</span></span>
    
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
                    "Result: {0}. <p>Partition key: '{1}' generated from hello first letter '{2}' of input value '{3}'. <br>Processing service partition ID: {4}. <br>Processing service replica address: {5}",
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
                output = output + "added tooPartition: " + primaryReplicaAddress;
                byte[] outBytes = Encoding.UTF8.GetBytes(output);
                response.OutputStream.Write(outBytes, 0, outBytes.Length);
            }
        }
    }
    ```
    
    <span data-ttu-id="6e932-254">Podívejme se krok za krokem.</span><span class="sxs-lookup"><span data-stu-id="6e932-254">Let's walk through it step by step.</span></span> <span data-ttu-id="6e932-255">Hello kód čte hello první písmeno parametr řetězce dotazu hello `lastname` do char.</span><span class="sxs-lookup"><span data-stu-id="6e932-255">hello code reads hello first letter of hello query string parameter `lastname` into a char.</span></span> <span data-ttu-id="6e932-256">Poté, určí hello klíč oddílu pro toto písmeno odečtením hello šestnáctkové hodnoty `A` z hello šestnáctkové hodnoty hello příjmení první písmeno.</span><span class="sxs-lookup"><span data-stu-id="6e932-256">Then, it determines hello partition key for this letter by subtracting hello hexadecimal value of `A` from hello hexadecimal value of hello last names' first letter.</span></span>
    
    ```CSharp
    string lastname = context.Request.QueryString["lastname"];
    char firstLetterOfLastName = lastname.First();
    ServicePartitionKey partitionKey = new ServicePartitionKey(Char.ToUpper(firstLetterOfLastName) - 'A');
    ```
    
    <span data-ttu-id="6e932-257">Pamatujte si, že v tomto příkladu že používáme 26 oddíly s klíčem jeden oddíl na oddíl.</span><span class="sxs-lookup"><span data-stu-id="6e932-257">Remember, for this example, we are using 26 partitions with one partition key per partition.</span></span>
    <span data-ttu-id="6e932-258">V dalším kroku získáme oddílu služby hello `partition` pro tento klíč pomocí hello `ResolveAsync` metodu hello `servicePartitionResolver` objektu.</span><span class="sxs-lookup"><span data-stu-id="6e932-258">Next, we obtain hello service partition `partition` for this key by using hello `ResolveAsync` method on hello `servicePartitionResolver` object.</span></span> <span data-ttu-id="6e932-259">`servicePartitionResolver`je definován jako</span><span class="sxs-lookup"><span data-stu-id="6e932-259">`servicePartitionResolver` is defined as</span></span>
    
    ```CSharp
    private readonly ServicePartitionResolver servicePartitionResolver = ServicePartitionResolver.GetDefault();
    ```
    
    <span data-ttu-id="6e932-260">Hello `ResolveAsync` URI metoda trvá hello služby, klíč oddílu hello a token zrušení jako parametry.</span><span class="sxs-lookup"><span data-stu-id="6e932-260">hello `ResolveAsync` method takes hello service URI, hello partition key, and a cancellation token as parameters.</span></span> <span data-ttu-id="6e932-261">Hello URI služby pro služby zpracování hello je `fabric:/AlphabetPartitions/Processing`.</span><span class="sxs-lookup"><span data-stu-id="6e932-261">hello service URI for hello processing service is `fabric:/AlphabetPartitions/Processing`.</span></span> <span data-ttu-id="6e932-262">V dalším kroku se nám získat koncový bod hello hello oddílu.</span><span class="sxs-lookup"><span data-stu-id="6e932-262">Next, we get hello endpoint of hello partition.</span></span>
    
    ```CSharp
    ResolvedServiceEndpoint ep = partition.GetEndpoint()
    ```
    
    <span data-ttu-id="6e932-263">Nakonec sestavit adresu URL koncového bodu hello plus hello řetězce dotazu a volání hello služby zpracování.</span><span class="sxs-lookup"><span data-stu-id="6e932-263">Finally, we build hello endpoint URL plus hello querystring and call hello processing service.</span></span>
    
    ```CSharp
    JObject addresses = JObject.Parse(ep.Address);
    string primaryReplicaAddress = (string)addresses["Endpoints"].First();
    
    UriBuilder primaryReplicaUriBuilder = new UriBuilder(primaryReplicaAddress);
    primaryReplicaUriBuilder.Query = "lastname=" + lastname;
    
    string result = await this.httpClient.GetStringAsync(primaryReplicaUriBuilder.Uri);
    ```
    
    <span data-ttu-id="6e932-264">Po dokončení zpracování hello výstup hello jsme zapsat zpět.</span><span class="sxs-lookup"><span data-stu-id="6e932-264">Once hello processing is done, we write hello output back.</span></span>
15. <span data-ttu-id="6e932-265">posledním krokem Hello je tootest hello služby.</span><span class="sxs-lookup"><span data-stu-id="6e932-265">hello last step is tootest hello service.</span></span> <span data-ttu-id="6e932-266">Parametry aplikace Visual Studio používá pro místní a cloudové nasazení.</span><span class="sxs-lookup"><span data-stu-id="6e932-266">Visual Studio uses application parameters for local and cloud deployment.</span></span> <span data-ttu-id="6e932-267">Služba hello tootest s 26 oddíly místně, je nutné tooupdate hello `Local.xml` souborů ve složce ApplicationParameters hello hello AlphabetPartitions projektu, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="6e932-267">tootest hello service with 26 partitions locally, you need tooupdate hello `Local.xml` file in hello ApplicationParameters folder of hello AlphabetPartitions project as shown below:</span></span>
    
    ```xml
    <Parameters>
      <Parameter Name="Processing_PartitionCount" Value="26" />
      <Parameter Name="WebApi_InstanceCount" Value="1" />
    </Parameters>
    ```
16. <span data-ttu-id="6e932-268">Po dokončení nasazení můžete zkontrolovat hello služby a všechny její oddíly v hello Service Fabric Exploreru.</span><span class="sxs-lookup"><span data-stu-id="6e932-268">Once you finish deployment, you can check hello service and all of its partitions in hello Service Fabric Explorer.</span></span>
    
    ![Snímek obrazovky Service Fabric Exploreru](./media/service-fabric-concepts-partitioning/sfxpartitions.png)
17. <span data-ttu-id="6e932-270">V prohlížeči, můžete otestovat hello dělení logiku zadáním `http://localhost:8081/?lastname=somename`.</span><span class="sxs-lookup"><span data-stu-id="6e932-270">In a browser, you can test hello partitioning logic by entering `http://localhost:8081/?lastname=somename`.</span></span> <span data-ttu-id="6e932-271">Zobrazí se, že každý příjmení začne s hello stejné písmeno ukládají do hello stejného oddílu.</span><span class="sxs-lookup"><span data-stu-id="6e932-271">You will see that each last name that starts with hello same letter is being stored in hello same partition.</span></span>
    
    ![Snímek obrazovky prohlížeče](./media/service-fabric-concepts-partitioning/samplerunning.png)

<span data-ttu-id="6e932-273">Ukázka hello Hello celý zdrojový kód je k dispozici na [Githubu](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/classic/Services/AlphabetPartitions).</span><span class="sxs-lookup"><span data-stu-id="6e932-273">hello entire source code of hello sample is available on [GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/classic/Services/AlphabetPartitions).</span></span>

## <a name="next-steps"></a><span data-ttu-id="6e932-274">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6e932-274">Next steps</span></span>
<span data-ttu-id="6e932-275">Informace o konceptech Service Fabric najdete v tématu hello následující:</span><span class="sxs-lookup"><span data-stu-id="6e932-275">For information on Service Fabric concepts, see hello following:</span></span>

* [<span data-ttu-id="6e932-276">Dostupnost služeb Service Fabric</span><span class="sxs-lookup"><span data-stu-id="6e932-276">Availability of Service Fabric services</span></span>](service-fabric-availability-services.md)
* [<span data-ttu-id="6e932-277">Škálovatelnost služby Service Fabric</span><span class="sxs-lookup"><span data-stu-id="6e932-277">Scalability of Service Fabric services</span></span>](service-fabric-concepts-scalability.md)
* [<span data-ttu-id="6e932-278">Plánování kapacity pro aplikace Service Fabric</span><span class="sxs-lookup"><span data-stu-id="6e932-278">Capacity planning for Service Fabric applications</span></span>](service-fabric-capacity-planning.md)

[wikipartition]: https://en.wikipedia.org/wiki/Partition_(database)

[1]: ./media/service-fabric-create-your-first-application-in-visual-studio/new-project-dialog-2.png