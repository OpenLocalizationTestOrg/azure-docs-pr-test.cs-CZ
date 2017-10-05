---
title: "Představení správce prostředků clusteru Service Fabric | Microsoft Docs"
description: "Úvod do Service Fabric clusteru správce prostředků."
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: cfab735b-923d-4246-a2a8-220d4f4e0c64
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: fd8d4eea239646a5d42955756e4f57742bb5ee4f
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="introducing-the-service-fabric-cluster-resource-manager"></a><span data-ttu-id="601c3-103">Představení správce prostředků clusteru Service Fabric</span><span class="sxs-lookup"><span data-stu-id="601c3-103">Introducing the Service Fabric cluster resource manager</span></span>
<span data-ttu-id="601c3-104">Tradičně Správa systémy IT nebo online služby určené vyhradit konkrétní fyzické nebo virtuální počítače pro tyto konkrétní služby nebo systémy.</span><span class="sxs-lookup"><span data-stu-id="601c3-104">Traditionally managing IT systems or online services meant dedicating specific physical or virtual machines to those specific services or systems.</span></span> <span data-ttu-id="601c3-105">Služby byly navržen jako vrstev.</span><span class="sxs-lookup"><span data-stu-id="601c3-105">Services were architected as tiers.</span></span> <span data-ttu-id="601c3-106">By "web" vrstvu a vrstvu "data" nebo "úložiště".</span><span class="sxs-lookup"><span data-stu-id="601c3-106">There would be a “web” tier and a “data” or “storage” tier.</span></span> <span data-ttu-id="601c3-107">Aplikace by měla mít zasílání zpráv úrovně, kde plynoucích požadavky a odhlašování, a také sada počítačů vyhrazený pro ukládání do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="601c3-107">Applications would have a messaging tier where requests flowed in and out, as well as a set of machines dedicated to caching.</span></span> <span data-ttu-id="601c3-108">Jednotlivé typy úlohy nebo vrstvě měl konkrétních počítačů, které jsou vyhrazené pro jeho: databáze získali vyhrazené, webové servery a několika několik počítačů.</span><span class="sxs-lookup"><span data-stu-id="601c3-108">Each tier or type of workload had specific machines dedicated to it: the database got a couple machines dedicated to it, the web servers a few.</span></span> <span data-ttu-id="601c3-109">Pokud konkrétní typ zatížení způsobená na počítače, které bylo na spuštění příliš aktivní, pak jste přidali další počítače pomocí této stejnou konfiguraci do této vrstvy.</span><span class="sxs-lookup"><span data-stu-id="601c3-109">If a particular type of workload caused the machines it was on to run too hot, then you added more machines with that same configuration to that tier.</span></span> <span data-ttu-id="601c3-110">Ale ne všechny úlohy může tak snadno škálovat – zejména s datovou vrstvu obvykle nahradíte počítače, které mají větší počítače.</span><span class="sxs-lookup"><span data-stu-id="601c3-110">However, not all workloads could be scaled out so easily - particularly with the data tier you would typically replace machines with larger machines.</span></span> <span data-ttu-id="601c3-111">Snadné.</span><span class="sxs-lookup"><span data-stu-id="601c3-111">Easy.</span></span> <span data-ttu-id="601c3-112">Pokud na počítači se nezdařilo, spustili část celkového aplikace na nižší kapacitu, dokud tento počítač může obnovit.</span><span class="sxs-lookup"><span data-stu-id="601c3-112">If a machine failed, that part of the overall application ran at lower capacity until the machine could be restored.</span></span> <span data-ttu-id="601c3-113">Stále poměrně snadno (Pokud je to nutně zábavné).</span><span class="sxs-lookup"><span data-stu-id="601c3-113">Still fairly easy (if not necessarily fun).</span></span>

<span data-ttu-id="601c3-114">Teď ale na světě architektury služby a software se změnila.</span><span class="sxs-lookup"><span data-stu-id="601c3-114">Now however the world of service and software architecture has changed.</span></span> <span data-ttu-id="601c3-115">Je dnes běžné, že aplikace přijaly návrh Škálováním na více systémů.</span><span class="sxs-lookup"><span data-stu-id="601c3-115">It's more common that applications have adopted a scale-out design.</span></span> <span data-ttu-id="601c3-116">Vytváření aplikací s kontejnery nebo mikroslužeb (nebo obě) je běžné.</span><span class="sxs-lookup"><span data-stu-id="601c3-116">Building applications with containers or microservices (or both) is common.</span></span> <span data-ttu-id="601c3-117">Teď když stále může mít pouze několik počítačů, neběží jenom jednu instanci zatížení.</span><span class="sxs-lookup"><span data-stu-id="601c3-117">Now, while you may still have only a few machines, they're not running just a single instance of a workload.</span></span> <span data-ttu-id="601c3-118">Budou pravděpodobně i spuštěna několik různých úloh ve stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="601c3-118">They may even be running multiple different workloads at the same time.</span></span> <span data-ttu-id="601c3-119">Nyní máte desítek různých typů služeb (none využívání úplné počítač za prostředky), případně stovky různých instancí těchto služeb.</span><span class="sxs-lookup"><span data-stu-id="601c3-119">You now have dozens of different types of services (none consuming a full machine's worth of resources), perhaps hundreds of different instances of those services.</span></span> <span data-ttu-id="601c3-120">Každé pojmenované instance má jeden nebo více instancí nebo replik pro vysokou dostupnost (HA).</span><span class="sxs-lookup"><span data-stu-id="601c3-120">Each named instance has one or more instances or replicas for High Availability (HA).</span></span> <span data-ttu-id="601c3-121">V závislosti na velikosti těchto zatížení, a jak jsou zaneprázdněny může najít sami se stovkami nebo tisíci počítačů.</span><span class="sxs-lookup"><span data-stu-id="601c3-121">Depending on the sizes of those workloads, and how busy they are, you may find yourself with hundreds or thousands of machines.</span></span> 

<span data-ttu-id="601c3-122">Náhle Správa prostředí není tak jednoduché, správu několika počítačů, které jsou vyhrazené pro jednotlivé typy úloh.</span><span class="sxs-lookup"><span data-stu-id="601c3-122">Suddenly managing your environment is not so simple as managing a few machines dedicated to single types of workloads.</span></span> <span data-ttu-id="601c3-123">Vaše servery jsou virtuální a už nebude mít názvy (Přepnuli jste mindsets z [mazlíčků k zvířat](http://www.slideshare.net/randybias/architectures-for-open-and-scalable-clouds/20) po všech).</span><span class="sxs-lookup"><span data-stu-id="601c3-123">Your servers are virtual and no longer have names (you have switched mindsets from [pets to cattle](http://www.slideshare.net/randybias/architectures-for-open-and-scalable-clouds/20) after all).</span></span> <span data-ttu-id="601c3-124">Konfigurace je menší o počítačích a informace o službách, sami.</span><span class="sxs-lookup"><span data-stu-id="601c3-124">Configuration is less about the machines and more about the services themselves.</span></span> <span data-ttu-id="601c3-125">Hardware, který je vyhrazen pro jednu instanci zatížení je do značné míry věcí minulosti.</span><span class="sxs-lookup"><span data-stu-id="601c3-125">Hardware that is dedicated to a single instance of a workload is largely a thing of the past.</span></span> <span data-ttu-id="601c3-126">Služby, sami staly malé distribuovaných systémů, které jsou rozmístěny v několika menší části komoditním hardwaru.</span><span class="sxs-lookup"><span data-stu-id="601c3-126">Services themselves have become small distributed systems that span multiple smaller pieces of commodity hardware.</span></span>

<span data-ttu-id="601c3-127">Vzhledem k tomu, že vaše aplikace není nadále řadu monoliths rozloženy několik vrstev, nyní máte mnoho více kombinací řešení.</span><span class="sxs-lookup"><span data-stu-id="601c3-127">Because your app is no longer a series of monoliths spread across several tiers, you now have many more combinations to deal with.</span></span> <span data-ttu-id="601c3-128">Kdo rozhodne, co můžete spustit typy úloh, na které hardwaru nebo kolik?</span><span class="sxs-lookup"><span data-stu-id="601c3-128">Who decides what types of workloads can run on which hardware, or how many?</span></span> <span data-ttu-id="601c3-129">Jaké úlohy dobré fungování i na stejném hardwaru, a který konfliktu?</span><span class="sxs-lookup"><span data-stu-id="601c3-129">Which workloads work well on the same hardware, and which conflict?</span></span> <span data-ttu-id="601c3-130">Kdy je počítač přestane dolů jak víte, co byla spuštěna existuje na tomto počítači?</span><span class="sxs-lookup"><span data-stu-id="601c3-130">When a machine goes down how do you know what was running there on that machine?</span></span> <span data-ttu-id="601c3-131">Kdo má na starosti a ujistěte se, že úlohy spustí znovu spustit?</span><span class="sxs-lookup"><span data-stu-id="601c3-131">Who is in charge of making sure that workload starts running again?</span></span> <span data-ttu-id="601c3-132">Počkejte (virtuální)? počítače opět online nebo úlohy automaticky převzetí služeb při selhání na jiné počítače a zachovat systémem?</span><span class="sxs-lookup"><span data-stu-id="601c3-132">Do you wait for the (virtual?) machine to come back or do your workloads automatically fail over to other machines and keep running?</span></span> <span data-ttu-id="601c3-133">Je potřeba lidského zásahu?</span><span class="sxs-lookup"><span data-stu-id="601c3-133">Is human intervention required?</span></span> <span data-ttu-id="601c3-134">Co upgrady pro toto prostředí?</span><span class="sxs-lookup"><span data-stu-id="601c3-134">What about upgrades in this environment?</span></span>

<span data-ttu-id="601c3-135">Jako vývojáři a operátory týkajících se v tomto prostředí vytvoříme má pomoci se správou této složitost.</span><span class="sxs-lookup"><span data-stu-id="601c3-135">As developers and operators dealing in this environment, we’re going to want help managing this complexity.</span></span> <span data-ttu-id="601c3-136">Náborové binge a při pokusu o překonávají složitost s uživateli, je pravděpodobně není pravé odpovědi, takže co můžeme udělat?</span><span class="sxs-lookup"><span data-stu-id="601c3-136">A hiring binge and trying to hide the complexity with people is probably not the right answer, so what do we do?</span></span>

## <a name="introducing-orchestrators"></a><span data-ttu-id="601c3-137">Představení orchestrators</span><span class="sxs-lookup"><span data-stu-id="601c3-137">Introducing orchestrators</span></span>
<span data-ttu-id="601c3-138">"Orchestrator" je obecný termín pro softwarového produktu, který pomáhá správcům spravovat tyto typy prostředí.</span><span class="sxs-lookup"><span data-stu-id="601c3-138">An “Orchestrator” is the general term for a piece of software that helps administrators manage these types of environments.</span></span> <span data-ttu-id="601c3-139">Orchestrators jsou komponenty, které trvat v žádostech o jako "Nastavit jako pět kopií této služby v mé prostředí."</span><span class="sxs-lookup"><span data-stu-id="601c3-139">Orchestrators are the components that take in requests like “I would like five copies of this service running in my environment."</span></span> <span data-ttu-id="601c3-140">Se pokusí provést v prostředí shodovat s požadovaným stavem, bez ohledu na to, co se stane.</span><span class="sxs-lookup"><span data-stu-id="601c3-140">They try to make the environment match the desired state, no matter what happens.</span></span>

<span data-ttu-id="601c3-141">Orchestrators (ne člověka) jsou co provést akci při selhání na počítači nebo zatížení ukončí neočekávané z nějakého důvodu.</span><span class="sxs-lookup"><span data-stu-id="601c3-141">Orchestrators (not humans) are what take action when a machine fails or a workload terminates for some unexpected reason.</span></span> <span data-ttu-id="601c3-142">Většina orchestrators více než jen nakládat s chybou.</span><span class="sxs-lookup"><span data-stu-id="601c3-142">Most orchestrators do more than just deal with failure.</span></span> <span data-ttu-id="601c3-143">Další funkce, které spravujete nová nasazení, upgrady zpracování a týkající se spotřeby prostředků a zásad správného řízení.</span><span class="sxs-lookup"><span data-stu-id="601c3-143">Other features they have are managing new deployments, handling upgrades, and dealing with resource consumption and governance.</span></span> <span data-ttu-id="601c3-144">Všechny orchestrators jsou zásadně o údržbu některé požadovaný stav konfigurace v prostředí.</span><span class="sxs-lookup"><span data-stu-id="601c3-144">All orchestrators are fundamentally about maintaining some desired state of configuration in the environment.</span></span> <span data-ttu-id="601c3-145">Chcete upozornit orchestrator mají a jej provést lifting náročné.</span><span class="sxs-lookup"><span data-stu-id="601c3-145">You want to be able to tell an orchestrator what you want and have it do the heavy lifting.</span></span> <span data-ttu-id="601c3-146">Všechny příklady orchestrators jsou programu Aurora nad Mesos, Docker Datacenter nebo Docker Swarm, Kubernetes a Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="601c3-146">Aurora on top of Mesos, Docker Datacenter/Docker Swarm, Kubernetes, and Service Fabric are all examples of orchestrators.</span></span> <span data-ttu-id="601c3-147">Tyto orchestrators jsou se aktivně vyvinuté tak, aby vyhovovaly skutečné úlohy v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="601c3-147">These orchestrators are being actively developed to meet the needs of real workloads in production environments.</span></span> 

## <a name="orchestration-as-a-service"></a><span data-ttu-id="601c3-148">Orchestration jako služby</span><span class="sxs-lookup"><span data-stu-id="601c3-148">Orchestration as a service</span></span>
<span data-ttu-id="601c3-149">Správce prostředků clusteru je součástí systému, který zpracovává orchestration v Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="601c3-149">The Cluster Resource Manager is the system component that handles orchestration in Service Fabric.</span></span> <span data-ttu-id="601c3-150">Úlohy správce prostředků clusteru je rozdělit do tří částí:</span><span class="sxs-lookup"><span data-stu-id="601c3-150">The Cluster Resource Manager’s job is broken down into three parts:</span></span>

1. <span data-ttu-id="601c3-151">Vynucení pravidla</span><span class="sxs-lookup"><span data-stu-id="601c3-151">Enforcing Rules</span></span>
2. <span data-ttu-id="601c3-152">Optimalizace prostředí</span><span class="sxs-lookup"><span data-stu-id="601c3-152">Optimizing Your Environment</span></span>
3. <span data-ttu-id="601c3-153">Pomoc s jinými procesy</span><span class="sxs-lookup"><span data-stu-id="601c3-153">Helping with Other Processes</span></span>

<span data-ttu-id="601c3-154">Jak funguje správce prostředků clusteru najdete v následujícím videu Microsoft Virtual Academy:<center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=d4tka66yC_5706218965">
<img src="./media/service-fabric-cluster-resource-manager-introduction/ConceptsAndDemoVid.png" WIDTH="360" HEIGHT="244">
</a></center></span><span class="sxs-lookup"><span data-stu-id="601c3-154">To see how the Cluster Resource Manager works, watch the following Microsoft Virtual Academy video: <center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=d4tka66yC_5706218965">
<img src="./media/service-fabric-cluster-resource-manager-introduction/ConceptsAndDemoVid.png" WIDTH="360" HEIGHT="244">
</a></center></span></span>

### <a name="what-it-isnt"></a><span data-ttu-id="601c3-155">Co není</span><span class="sxs-lookup"><span data-stu-id="601c3-155">What it isn’t</span></span>
<span data-ttu-id="601c3-156">V aplikacích vrstvy tradiční N, je vždy [nástroj pro vyrovnávání zatížení](https://en.wikipedia.org/wiki/Load_balancing_(computing)).</span><span class="sxs-lookup"><span data-stu-id="601c3-156">In traditional N tier applications, there's always a [Load Balancer](https://en.wikipedia.org/wiki/Load_balancing_(computing)).</span></span> <span data-ttu-id="601c3-157">Obvykle se to nástroj pro vyrovnávání zatížení sítě (NLB) nebo aplikaci zatížení vyrovnávání (ALB) v závislosti na tom, kde ji byla ve sadu síťových protokolů.</span><span class="sxs-lookup"><span data-stu-id="601c3-157">Usually this was a Network Load Balancer (NLB) or an Application Load Balancer (ALB) depending on where it sat in the networking stack.</span></span> <span data-ttu-id="601c3-158">Některé nástroje pro vyrovnávání zatížení jsou založené na hardwaru jako je F5 Big-IP nabídky, jiné jsou na základě softwaru jako je Microsoft je služba Vyrovnávání zatížení sítě.</span><span class="sxs-lookup"><span data-stu-id="601c3-158">Some load balancers are Hardware-based like F5’s BigIP offering, others are software-based such as Microsoft’s NLB.</span></span> <span data-ttu-id="601c3-159">V jiných prostředích může se zobrazit něco jako HAProxy, nginx, Istio nebo Envoy v této roli.</span><span class="sxs-lookup"><span data-stu-id="601c3-159">In other environments, you might see something like HAProxy, nginx, Istio, or Envoy in this role.</span></span> <span data-ttu-id="601c3-160">V případě architektur se tyto úlohy služby Vyrovnávání zatížení je zajistit Bezstavová zatížení přijímat (přibližně) stejné množství práce.</span><span class="sxs-lookup"><span data-stu-id="601c3-160">In these architectures, the job of load balancing is to ensure stateless workloads receive (roughly) the same amount of work.</span></span> <span data-ttu-id="601c3-161">Strategie pro vyrovnávání zatížení rozmanitých.</span><span class="sxs-lookup"><span data-stu-id="601c3-161">Strategies for balancing load varied.</span></span> <span data-ttu-id="601c3-162">Některé nástroje pro vyrovnávání byste odesílali každý jiný volání na jiný server.</span><span class="sxs-lookup"><span data-stu-id="601c3-162">Some balancers would send each different call to a different server.</span></span> <span data-ttu-id="601c3-163">Ostatní zadat Připnutí/dlouhodobost relace.</span><span class="sxs-lookup"><span data-stu-id="601c3-163">Others provided session pinning/stickiness.</span></span> <span data-ttu-id="601c3-164">Pokročilejší vyrovnávání používat ke směrování na základě jejího očekávané náklady a aktuálního zatížení počítače volání vlastní operaci načtení odhad nebo generování sestav.</span><span class="sxs-lookup"><span data-stu-id="601c3-164">More advanced balancers use actual load estimation or reporting to route a call based on its expected cost and current machine load.</span></span>

<span data-ttu-id="601c3-165">Nástroje pro vyrovnávání sítě nebo zprávy směrovače se pokusila o Ujistěte se, že zůstal zhruba vyrovnáváním vrstvě web nebo worker.</span><span class="sxs-lookup"><span data-stu-id="601c3-165">Network balancers or message routers tried to ensure that the web/worker tier remained roughly balanced.</span></span> <span data-ttu-id="601c3-166">Strategie pro datové vrstvy vyrovnávání měla jiné a závislé na mechanizmus pro ukládání dat.</span><span class="sxs-lookup"><span data-stu-id="601c3-166">Strategies for balancing the data tier were different and depended on the data storage mechanism.</span></span> <span data-ttu-id="601c3-167">Datová vrstva vyrovnávání účinné horizontálního dělení dat, ukládání do mezipaměti, spravované zobrazení, uložené procedury a další mechanismy daného obchodu.</span><span class="sxs-lookup"><span data-stu-id="601c3-167">Balancing the data tier relied on data sharding, caching, managed views, stored procedures, and other store-specific mechanisms.</span></span>

<span data-ttu-id="601c3-168">Některé z těchto strategií jsou zajímavé, správce prostředků clusteru Service Fabric není nic jako nástroj pro vyrovnávání zatížení sítě nebo mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="601c3-168">While some of these strategies are interesting, the Service Fabric Cluster Resource Manager is not anything like a network load balancer or a cache.</span></span> <span data-ttu-id="601c3-169">Nástroj pro vyrovnávání zatížení sítě vyrovnává frontends tak, že se provoz mezi frontends.</span><span class="sxs-lookup"><span data-stu-id="601c3-169">A Network Load Balancer balances frontends by spreading traffic across frontends.</span></span> <span data-ttu-id="601c3-170">Správce prostředků clusteru Service Fabric má jinou strategii.</span><span class="sxs-lookup"><span data-stu-id="601c3-170">The Service Fabric Cluster Resource Manager has a different strategy.</span></span> <span data-ttu-id="601c3-171">Zásadně, Service Fabric přesune *služby* kde udělat nejvhodnější, byla očekávána provoz, nebo podle zatížení.</span><span class="sxs-lookup"><span data-stu-id="601c3-171">Fundamentally, Service Fabric moves *services* to where they make the most sense, expecting traffic or load to follow.</span></span> <span data-ttu-id="601c3-172">Například je může přesunout služby do uzlů, které jsou aktuálně studenou, protože nejsou služby, které jsou to množství práce.</span><span class="sxs-lookup"><span data-stu-id="601c3-172">For example, it might move services to nodes that are currently cold because the services that are there are not doing much work.</span></span> <span data-ttu-id="601c3-173">Uzly může být studené vzhledem k tomu, že byly odstraněny nebo přesunout jinam služby, které nebyly nalezeny.</span><span class="sxs-lookup"><span data-stu-id="601c3-173">The nodes may be cold since the services that were present were deleted or moved elsewhere.</span></span> <span data-ttu-id="601c3-174">Například může správce prostředků clusteru služby od počítače s také přesunout.</span><span class="sxs-lookup"><span data-stu-id="601c3-174">As another example, the Cluster Resource Manager could also move a service away from a machine.</span></span> <span data-ttu-id="601c3-175">Možná se tento počítač je budou upgradovány, nebo je z důvodu Špička při spotřebě přetížené se služby spuštěné na něm.</span><span class="sxs-lookup"><span data-stu-id="601c3-175">Perhaps the machine is about to be upgraded, or is overloaded due to a spike in consumption by the services running on it.</span></span> <span data-ttu-id="601c3-176">Alernatively, může mít vyšší požadavky na služby prostředků.</span><span class="sxs-lookup"><span data-stu-id="601c3-176">Alernatively, the service's resource requirements may have increased.</span></span> <span data-ttu-id="601c3-177">V důsledku nejsou k dispozici dostatečné prostředky na tomto počítači spusťte jej.</span><span class="sxs-lookup"><span data-stu-id="601c3-177">As a result there aren't sufficient resources on this machine to continue running it.</span></span> 

<span data-ttu-id="601c3-178">Vzhledem k tomu, že správce prostředků clusteru je zodpovědná za přesunutí služby kolem, obsahuje sadu různé funkce ve srovnání s by najít ve službě Vyrovnávání zatížení sítě.</span><span class="sxs-lookup"><span data-stu-id="601c3-178">Because the Cluster Resource Manager is responsible for moving services around, it contains a different feature set compared to what you would find in a network load balancer.</span></span> <span data-ttu-id="601c3-179">Toto je nástroje pro vyrovnávání zatížení sítě přinášejí síťové přenosy k umístění služby již, i když toto umístění není ideální pro spouštění samotnou službu.</span><span class="sxs-lookup"><span data-stu-id="601c3-179">This is because network load balancers deliver network traffic to where services already are, even if that location is not ideal for running the service itself.</span></span> <span data-ttu-id="601c3-180">Správce prostředků clusteru Service Fabric aktivuje se podstatně liší strategie pro zajištění, že jsou efektivně využité prostředky v clusteru.</span><span class="sxs-lookup"><span data-stu-id="601c3-180">The Service Fabric Cluster Resource Manager employs fundamentally different strategies for ensuring that the resources in the cluster are efficiently utilized.</span></span>

## <a name="next-steps"></a><span data-ttu-id="601c3-181">Další kroky</span><span class="sxs-lookup"><span data-stu-id="601c3-181">Next steps</span></span>
- <span data-ttu-id="601c3-182">Informace o architektuře a informačního toku v rámci správce prostředků clusteru, podívejte se na [v tomto článku](service-fabric-cluster-resource-manager-architecture.md)</span><span class="sxs-lookup"><span data-stu-id="601c3-182">For information on the architecture and information flow within the Cluster Resource Manager, check out [this article ](service-fabric-cluster-resource-manager-architecture.md)</span></span>
- <span data-ttu-id="601c3-183">Správce prostředků clusteru má mnoho možností pro popis clusteru.</span><span class="sxs-lookup"><span data-stu-id="601c3-183">The Cluster Resource Manager has many options for describing the cluster.</span></span> <span data-ttu-id="601c3-184">Chcete-li získat další informace o metriky, projděte si tento článek na [popisující cluster Service Fabric](service-fabric-cluster-resource-manager-cluster-description.md)</span><span class="sxs-lookup"><span data-stu-id="601c3-184">To find out more about metrics, check out this article on [describing a Service Fabric cluster](service-fabric-cluster-resource-manager-cluster-description.md)</span></span>
- <span data-ttu-id="601c3-185">Další informace o konfiguraci služby [Další informace o konfiguraci služby](service-fabric-cluster-resource-manager-configure-services.md)(service-fabric-cluster-resource-manager-configure-services.md)</span><span class="sxs-lookup"><span data-stu-id="601c3-185">For more information on configuring services, [Learn about configuring Services](service-fabric-cluster-resource-manager-configure-services.md)(service-fabric-cluster-resource-manager-configure-services.md)</span></span>
- <span data-ttu-id="601c3-186">Metriky se, jak správce prostředků služby Fabric clusteru spravuje využívání a kapacity v clusteru.</span><span class="sxs-lookup"><span data-stu-id="601c3-186">Metrics are how the Service Fabric Cluster Resource Manger manages consumption and capacity in the cluster.</span></span> <span data-ttu-id="601c3-187">Další informace o metriky a způsob jejich konfigurace rezervaci [v tomto článku](service-fabric-cluster-resource-manager-metrics.md)</span><span class="sxs-lookup"><span data-stu-id="601c3-187">To learn more about metrics and how to configure them check out [this article](service-fabric-cluster-resource-manager-metrics.md)</span></span>
- <span data-ttu-id="601c3-188">Správce prostředků clusteru pracuje s možností správy Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="601c3-188">The Cluster Resource Manager works with Service Fabric's management capabilities.</span></span> <span data-ttu-id="601c3-189">Chcete-li získat další informace o této integrace, přečtěte si [v tomto článku](service-fabric-cluster-resource-manager-management-integration.md)</span><span class="sxs-lookup"><span data-stu-id="601c3-189">To find out more about that integration, read [this article](service-fabric-cluster-resource-manager-management-integration.md)</span></span>
- <span data-ttu-id="601c3-190">Chcete-li zjistit, o tom, jak správce prostředků clusteru spravuje a vyrovnává zatížení v clusteru, podívejte se na článek na [Vyrovnávání zatížení](service-fabric-cluster-resource-manager-balancing.md)</span><span class="sxs-lookup"><span data-stu-id="601c3-190">To find out about how the Cluster Resource Manager manages and balances load in the cluster, check out the article on [balancing load](service-fabric-cluster-resource-manager-balancing.md)</span></span>