---
title: Popis clusteru clusteru Resource Manager | Microsoft Docs
description: "Cluster Service Fabric popisující zadáním domén selhání, Upgrade domény, vlastnosti uzlu a uzel kapacity pro správce prostředků clusteru."
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: 55f8ab37-9399-4c9a-9e6c-d2d859de6766
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: e517eda4d3ff7ad81998003688c3cca78f76e179
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="describing-a-service-fabric-cluster"></a><span data-ttu-id="6a2f6-103">Popisující service fabric cluster</span><span class="sxs-lookup"><span data-stu-id="6a2f6-103">Describing a service fabric cluster</span></span>
<span data-ttu-id="6a2f6-104">Služba Fabric clusteru Resource Manager poskytuje několik mechanismů pro popis clusteru.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-104">The Service Fabric Cluster Resource Manager provides several mechanisms for describing a cluster.</span></span> <span data-ttu-id="6a2f6-105">Během doby běhu správce prostředků clusteru používá tyto informace k zajištění vysoké dostupnosti se služby spuštěné v clusteru.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-105">During runtime, the Cluster Resource Manager uses this information to ensure high availability of the services running in the cluster.</span></span> <span data-ttu-id="6a2f6-106">Při vynucování tyto důležité pravidla, je taky automatický pokus o optimalizovat spotřeby prostředků v rámci clusteru.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-106">While enforcing these important rules, it also attempts to optimize the resource consumption within the cluster.</span></span>

## <a name="key-concepts"></a><span data-ttu-id="6a2f6-107">Klíčové koncepty</span><span class="sxs-lookup"><span data-stu-id="6a2f6-107">Key concepts</span></span>
<span data-ttu-id="6a2f6-108">Správce prostředků clusteru podporuje několik funkcí, které popisují clusteru:</span><span class="sxs-lookup"><span data-stu-id="6a2f6-108">The Cluster Resource Manager supports several features that describe a cluster:</span></span>

* <span data-ttu-id="6a2f6-109">Domén selhání</span><span class="sxs-lookup"><span data-stu-id="6a2f6-109">Fault Domains</span></span>
* <span data-ttu-id="6a2f6-110">Domén upgradu</span><span class="sxs-lookup"><span data-stu-id="6a2f6-110">Upgrade Domains</span></span>
* <span data-ttu-id="6a2f6-111">Vlastnosti uzlu</span><span class="sxs-lookup"><span data-stu-id="6a2f6-111">Node Properties</span></span>
* <span data-ttu-id="6a2f6-112">Uzel kapacity</span><span class="sxs-lookup"><span data-stu-id="6a2f6-112">Node Capacities</span></span>

## <a name="fault-domains"></a><span data-ttu-id="6a2f6-113">Domény selhání</span><span class="sxs-lookup"><span data-stu-id="6a2f6-113">Fault domains</span></span>
<span data-ttu-id="6a2f6-114">Domény selhání je všechny oblasti koordinované selhání.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-114">A Fault Domain is any area of coordinated failure.</span></span> <span data-ttu-id="6a2f6-115">Jeden počítač je domény selhání (protože ho může selhat v jeho vlastní pro různé příčiny selhání power napájení na jednotce selhání chybný firmwaru síťový adaptér).</span><span class="sxs-lookup"><span data-stu-id="6a2f6-115">A single machine is a Fault Domain (since it can fail on its own for various reasons, from power supply failures to drive failures to bad NIC firmware).</span></span> <span data-ttu-id="6a2f6-116">Počítače připojené ke stejnému přepínači sítě Ethernet jsou ve stejné doméně selhání, jako jsou počítače sdílení jednoho zdroje napájení nebo na jednom místě.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-116">Machines connected to the same Ethernet switch are in the same Fault Domain, as are machines sharing a single source of power or in a single location.</span></span> <span data-ttu-id="6a2f6-117">Vzhledem k tomu, že je přirozené pro chyby hardwaru na překrývají, jsou ze své podstaty hierarchická domén selhání a jsou reprezentovány jako identifikátory URI v Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-117">Since it's natural for hardware faults to overlap, Fault Domains are inherently hierarchal and are represented as URIs in Service Fabric.</span></span>

<span data-ttu-id="6a2f6-118">Je důležité, aby domén selhání jsou správně nastaveny vzhledem k tomu, že Service Fabric používá tuto informaci k bezpečně umístění služby.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-118">It is important that Fault Domains are set up correctly since Service Fabric uses this information to safely place services.</span></span> <span data-ttu-id="6a2f6-119">Service Fabric nechce umístit služby tak, aby způsobí ztrátu domény selhání (způsobené selhání některé komponenty) služba přejděte.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-119">Service Fabric doesn't want to place services such that the loss of a Fault Domain (caused by the failure of some component) causes a service to go down.</span></span> <span data-ttu-id="6a2f6-120">Ve službě Azure prostředí Service Fabric používá poskytnuté prostředím informace o doméně selhání správně nakonfigurovat uzly v clusteru vaším jménem.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-120">In the Azure environment Service Fabric uses the Fault Domain information provided by the environment to correctly configure the nodes in the cluster on your behalf.</span></span> <span data-ttu-id="6a2f6-121">Pro samostatnou službu prostředků infrastruktury domén selhání jsou definovány v čase, který je nastavený clusteru</span><span class="sxs-lookup"><span data-stu-id="6a2f6-121">For Service Fabric Standalone, Fault Domains are defined at the time that the cluster is set up</span></span> 

> [!WARNING]
> <span data-ttu-id="6a2f6-122">Je důležité, aby doména selhání informací uvedených na Service Fabric přesná.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-122">It is important that the Fault Domain information provided to Service Fabric is accurate.</span></span> <span data-ttu-id="6a2f6-123">Například předpokládejme, že uzly clusteru Service Fabric běží uvnitř 10 virtuálních počítačů běžících na pět fyzických hostitelích.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-123">For example, let's say that your Service Fabric cluster's nodes are running inside 10 virtual machines, running on five physical hosts.</span></span> <span data-ttu-id="6a2f6-124">V takovém případě i když se 10 virtuálních počítačů, je jenom 5 různých (nejvyšší úrovně) poruch domén.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-124">In this case, even though there are 10 virtual machines, there's only 5 different (top level) fault domains.</span></span> <span data-ttu-id="6a2f6-125">Sdílení na stejném fyzickém hostiteli způsobí, že virtuální počítače sdílet stejné kořenové domény selhání vzhledem k tomu, že virtuální počítače zaznamenat koordinované selhání, pokud se nezdaří jejich fyzického hostitele.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-125">Sharing the same physical host causes VMs to share the same root fault domain, since the VMs experience coordinated failure if their physical host fails.</span></span>  
>
> <span data-ttu-id="6a2f6-126">Vzhledem k tomu, že Service Fabric očekává domény selhání uzlu nechcete změnit.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-126">Since Service Fabric expects the Fault Domain of a node not to change.</span></span> <span data-ttu-id="6a2f6-127">Další mechanismy pro zajištění vysoké dostupnosti virtuálních počítačů, jako například [HA – VMs](https://technet.microsoft.com/en-us/library/cc967323.aspx), použijte transparentní migraci virtuálních počítačů z jednoho hostitele na druhého.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-127">Other mechanisms of ensuring high availability of the VMs, such as [HA-VMs](https://technet.microsoft.com/en-us/library/cc967323.aspx), use transparent migration of VMs from one host to another.</span></span> <span data-ttu-id="6a2f6-128">Tyto mechanismy nezadávejte překonfigurovat nebo oznámit kód spuštěný ve virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-128">These mechanisms do not reconfigure or notify the running code inside the VM.</span></span> <span data-ttu-id="6a2f6-129">Jako takový jsou **nepodporuje** jako prostředí pro spuštění Service Fabric clusterů.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-129">As such, they are **not supported** as environments for running Service Fabric clusters.</span></span> <span data-ttu-id="6a2f6-130">Service Fabric musí být používané technologie pouze vysokou dostupnost.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-130">Service Fabric should be the only high-availability technology employed.</span></span> <span data-ttu-id="6a2f6-131">Mechanismy, jako je migrace za provozu virtuálního počítače, sítě SAN, nebo jiné nejsou potřebné.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-131">Mechanisms like live VM migration, SANs, or others are not necessary.</span></span> <span data-ttu-id="6a2f6-132">Pokud se používá ve spojení s Service Fabric, tyto mechanismy _snížit_ aplikace dostupnost a spolehlivost vzhledem k tomu, že zavést další složitosti, přidejte centralizované zdroje selhání a využívat spolehlivost a strategie dostupnosti, které je v konfliktu s těmi, která v Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-132">If used in conjunction with Service Fabric, these mechanisms _reduce_ application availability and reliability because they introduce additional complexity, add centralized sources of failure, and utilize reliability and availability strategies that conflict with those in Service Fabric.</span></span> 
>
>

<span data-ttu-id="6a2f6-133">Na následujícím obrázku jsme barva všech entit, které seznam všech různé chyby domén, které způsobit a přispívat do domény selhání.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-133">In the graphic below we color all the entities that contribute to Fault Domains and list all the different Fault Domains that result.</span></span> <span data-ttu-id="6a2f6-134">V tomto příkladu máme datových centrech (dále jen "řadiče domény"), stojany ("R") a okna ("B").</span><span class="sxs-lookup"><span data-stu-id="6a2f6-134">In this example, we have datacenters ("DC"), racks ("R"), and blades ("B").</span></span> <span data-ttu-id="6a2f6-135">V opačném případě pokud každý okno obsahuje více než jeden virtuální počítač, může dojít další úroveň v hierarchii domény selhání.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-135">Conceivably, if each blade holds more than one virtual machine, there could be another layer in the Fault Domain hierarchy.</span></span>

<span data-ttu-id="6a2f6-136"><center>
![Uzly uspořádané prostřednictvím domén selhání][Image1]
</center></span><span class="sxs-lookup"><span data-stu-id="6a2f6-136"><center>
![Nodes organized via Fault Domains][Image1]
</center></span></span>

<span data-ttu-id="6a2f6-137">Během doby běhu správce prostředků clusteru Service Fabric zvažuje domén selhání v clusteru a plány rozložení.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-137">During runtime, the Service Fabric Cluster Resource Manager considers the Fault Domains in the cluster and plans layouts.</span></span> <span data-ttu-id="6a2f6-138">Stavová repliky nebo bezstavové instance pro danou službu se distribuují tak, aby byly v samostatných doménách selhání.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-138">The stateful replicas or stateless instances for a given service are distributed so they are in separate Fault Domains.</span></span> <span data-ttu-id="6a2f6-139">Distribuce služby napříč doménami selhání zajistí, že nedojde k ohrožení dostupnost služby při selhání doména selhání na libovolné úrovni hierarchie.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-139">Distributing the service across fault domains ensures the availability of the service is not compromised when a Fault Domain fails at any level of the hierarchy.</span></span>

<span data-ttu-id="6a2f6-140">Správce prostředků clusteru Service Fabric není v potaz, kolik vrstvy, které jsou v hierarchii domény selhání.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-140">Service Fabric’s Cluster Resource Manager doesn’t care how many layers there are in the Fault Domain hierarchy.</span></span> <span data-ttu-id="6a2f6-141">Ale pokusí se ujistěte, že ke ztrátě všech jednu část hierarchie nebude mít vliv na běžící v ní.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-141">However, it tries to ensure that the loss of any one portion of the hierarchy doesn’t impact services running in it.</span></span> 

<span data-ttu-id="6a2f6-142">Je nejvhodnější při stejný počet uzlů na každé úrovni hloubka v hierarchii domény selhání.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-142">It is best if there are the same number of nodes at each level of depth in the Fault Domain hierarchy.</span></span> <span data-ttu-id="6a2f6-143">Pokud je "stromu" domén selhání nevyváženou v clusteru, obtížnější pro Resource Manager clusteru a pokuste se zjistit doporučené přidělení služeb.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-143">If the “tree” of Fault Domains is unbalanced in your cluster, it makes it harder for the Cluster Resource Manager to figure out the best allocation of services.</span></span> <span data-ttu-id="6a2f6-144">Imbalanced rozložení domén selhání znamenají, že ke ztrátě některých domén dopadu na dostupnost služeb víc než jiných doménách.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-144">Imbalanced Fault Domains layouts mean that the loss of some domains impact the availability of services more than other domains.</span></span> <span data-ttu-id="6a2f6-145">V důsledku toho odpojí správce prostředků clusteru mezi dva cíle: chce použít na počítače v této doméně "těžká" na jejich umístění služby a chce umístit služby v jiných doménách tak, aby ztrátě domény není způsobit problémy.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-145">As a result, the Cluster Resource Manager is torn between two goals: It wants to use the machines in that “heavy” domain by placing services on them, and it wants to place services in other domains so that the loss of a domain doesn’t cause problems.</span></span> 

<span data-ttu-id="6a2f6-146">Co imbalanced domén vypadat?</span><span class="sxs-lookup"><span data-stu-id="6a2f6-146">What do imbalanced domains look like?</span></span> <span data-ttu-id="6a2f6-147">V následujícím diagramu ukážeme dvě rozložení jiného clusteru.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-147">In the diagram below, we show two different cluster layouts.</span></span> <span data-ttu-id="6a2f6-148">V prvním příkladu jsou rovnoměrně rozloženy uzly na domén selhání.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-148">In the first example, the nodes are distributed evenly across the Fault Domains.</span></span> <span data-ttu-id="6a2f6-149">V druhém příkladu má jeden doména selhání mnoho více uzlů, než v jiných doménách selhání.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-149">In the second example, one Fault Domain has many more nodes than the other Fault Domains.</span></span> 

<span data-ttu-id="6a2f6-150"><center>
![Dvě různé clusteru rozložení][Image2]
</center></span><span class="sxs-lookup"><span data-stu-id="6a2f6-150"><center>
![Two different cluster layouts][Image2]
</center></span></span>

<span data-ttu-id="6a2f6-151">V Azure výběr z nich obsahuje doména selhání uzlu je spravovaná.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-151">In Azure, the choice of which Fault Domain contains a node is managed for you.</span></span> <span data-ttu-id="6a2f6-152">Ale v závislosti na počtu uzlů, které můžete zřídit můžete stále skončili s domén selhání s více uzly v nich než jiné.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-152">However, depending on the number of nodes that you provision you can still end up with Fault Domains with more nodes in them than others.</span></span> <span data-ttu-id="6a2f6-153">Předpokládejme například že máte pět domén selhání v clusteru, ale zřídit sedmi uzlů pro daný typ uzlu.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-153">For example, say you have five Fault Domains in the cluster but provision seven nodes for a given NodeType.</span></span> <span data-ttu-id="6a2f6-154">V takovém případě první dva domén selhání skončili s více uzly.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-154">In this case, the first two Fault Domains end up with more nodes.</span></span> <span data-ttu-id="6a2f6-155">Pokud budete pokračovat v nasazování další NodeTypes s pouze několik instancí, získá zhoršení problém.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-155">If you continue to deploy more NodeTypes with only a couple instances, the problem gets worse.</span></span> <span data-ttu-id="6a2f6-156">Z toho důvodu, který má doporučeno počet uzlů v každém uzlu typ je násobkem počet domén selhání.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-156">For this reason it's recommended that the number of nodes in each node type is a multiple of the number of Fault Domains.</span></span>

## <a name="upgrade-domains"></a><span data-ttu-id="6a2f6-157">Domén upgradu</span><span class="sxs-lookup"><span data-stu-id="6a2f6-157">Upgrade domains</span></span>
<span data-ttu-id="6a2f6-158">Upgradu domény jsou další funkce, která pomáhá pochopit rozložení clusteru správce prostředků clusteru Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-158">Upgrade Domains are another feature that helps the Service Fabric Cluster Resource Manager understand the layout of the cluster.</span></span> <span data-ttu-id="6a2f6-159">Domén upgradu definovat sadu uzlů, které jsou upgradovány na stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-159">Upgrade Domains define sets of nodes that are upgraded at the same time.</span></span> <span data-ttu-id="6a2f6-160">Domén upgradu pomoct pochopit a orchestraci management operace, jako je upgrady správce prostředků clusteru.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-160">Upgrade Domains help the Cluster Resource Manager understand and orchestrate management operations like upgrades.</span></span>

<span data-ttu-id="6a2f6-161">Upgradu domény jsou mnohem jako domén selhání, ale s několik hlavní rozdíly.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-161">Upgrade Domains are a lot like Fault Domains, but with a couple key differences.</span></span> <span data-ttu-id="6a2f6-162">Nejprve oblasti selhání koordinované hardwaru definování domén selhání.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-162">First, areas of coordinated hardware failures define Fault Domains.</span></span> <span data-ttu-id="6a2f6-163">Zásady jsou definované upgradu domény, na druhé straně.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-163">Upgrade Domains, on the other hand, are defined by policy.</span></span> <span data-ttu-id="6a2f6-164">Bude docházet k rozhodování o tom, kolik chcete, místo ho se závisí na prostředí.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-164">You get to decide how many you want, rather than it being dictated by the environment.</span></span> <span data-ttu-id="6a2f6-165">Můžete mít tolik upgradu domény jako uzly.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-165">You could have as many Upgrade Domains as you do nodes.</span></span> <span data-ttu-id="6a2f6-166">Další rozdíl mezi domén selhání a upgradu domény je, že Upgrade domény nejsou hierarchické.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-166">Another difference between Fault Domains and Upgrade Domains is that Upgrade Domains are not hierarchical.</span></span> <span data-ttu-id="6a2f6-167">Místo toho se více podobají prostřednictvím jednoduchých značek.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-167">Instead, they are more like a simple tag.</span></span> 

<span data-ttu-id="6a2f6-168">Následující diagram znázorňuje, že jsou tři domény upgradu rozdělené mezi tři domény selhání.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-168">The following diagram shows three Upgrade Domains are striped across three Fault Domains.</span></span> <span data-ttu-id="6a2f6-169">Také ukazuje jeden možné umístění pro tři různé repliky stavové služby, kde každý skončilo v různých selhání a upgradu domény.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-169">It also shows one possible placement for three different replicas of a stateful service, where each ends up in different Fault and Upgrade Domains.</span></span> <span data-ttu-id="6a2f6-170">Toto umístění umožňuje ztrátu domény selhání při uprostřed upgrade služby a ještě jednu kopii kód a data.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-170">This placement allows the loss of a Fault Domain while in the middle of a service upgrade and still have one copy of the code and data.</span></span>  

<span data-ttu-id="6a2f6-171"><center>
![Umístění s doménách selhání a upgradu][Image3]
</center></span><span class="sxs-lookup"><span data-stu-id="6a2f6-171"><center>
![Placement With Fault and Upgrade Domains][Image3]
</center></span></span>

<span data-ttu-id="6a2f6-172">Existují výhody a nevýhody na situaci, kdy velký počet domén upgradu.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-172">There are pros and cons to having large numbers of Upgrade Domains.</span></span> <span data-ttu-id="6a2f6-173">Další domény upgradu znamená každého kroku upgradu je podrobnější a ovlivňuje menší počet uzlů nebo služby.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-173">More Upgrade Domains means each step of the upgrade is more granular and therefore affects a smaller number of nodes or services.</span></span> <span data-ttu-id="6a2f6-174">V důsledku toho méně služby mají přesunout současně, představení méně změn do systému.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-174">As a result, fewer services have to move at a time, introducing less churn into the system.</span></span> <span data-ttu-id="6a2f6-175">To může zvýšit spolehlivost, protože méně služby je ovlivněn jakýkoli problém zavedená během upgradu.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-175">This tends to improve reliability, since less of the service is impacted by any issue introduced during the upgrade.</span></span> <span data-ttu-id="6a2f6-176">Další domény upgradu také znamená, je nutné, aby méně dostupné vyrovnávací paměti na jiných uzlech pro zpracování dopad upgradu.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-176">More Upgrade Domains also means that you need less available buffer on other nodes to handle the impact of the upgrade.</span></span> <span data-ttu-id="6a2f6-177">Například pokud máte pět upgradu domén, uzly v každé jsou zpracování zhruba 20 % z provozu.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-177">For example, if you have five Upgrade Domains, the nodes in each are handling roughly 20% of your traffic.</span></span> <span data-ttu-id="6a2f6-178">Pokud potřebujete vypnout upgradu domény pro účely upgradu, aby byla zátěž obvykle musí někde přejděte.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-178">If you need to take down that Upgrade Domain for an upgrade, that load usually needs to go somewhere.</span></span> <span data-ttu-id="6a2f6-179">Vzhledem k tomu, že máte čtyři zbývající domény upgradu, každý musí mít dostatek místa pro přibližně 5 % celkového provozu.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-179">Since you have four remaining Upgrade Domains, each must have room for about 5% of the total traffic.</span></span> <span data-ttu-id="6a2f6-180">Další domény upgradu znamená, že budete potřebovat méně vyrovnávací paměť v uzlech v clusteru.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-180">More Upgrade Domains means you need less buffer on the nodes in the cluster.</span></span> <span data-ttu-id="6a2f6-181">Zvažte například, pokud jste měli 10 upgradu domén místo.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-181">For example, consider if you had 10 Upgrade Domains instead.</span></span> <span data-ttu-id="6a2f6-182">V takovém případě každý UD by pouze zpracovávat přibližně 10 % celkového provozu.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-182">In that case, each UD would only be handling about 10% of the total traffic.</span></span> <span data-ttu-id="6a2f6-183">Při upgradu kroky prostřednictvím clusteru, třeba, aby měl prostor pro přibližně 1.1 % celkového provozu pouze každou doménu.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-183">When an upgrade steps through the cluster, each domain would only need to have room for about 1.1% of the total traffic.</span></span> <span data-ttu-id="6a2f6-184">Další domény upgradu obecně umožňuje spouštět uzly s vyšší využití, vzhledem k tomu, že budete potřebovat méně rezervované kapacity.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-184">More Upgrade Domains generally allow you to run your nodes at higher utilization, since you need less reserved capacity.</span></span> <span data-ttu-id="6a2f6-185">Totéž platí pro domény selhání.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-185">The same is true for Fault Domains.</span></span>  

<span data-ttu-id="6a2f6-186">Nevýhodou, že máte velký počet domén upgradu je, že upgrady mívají trvá déle.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-186">The downside of having many Upgrade Domains is that upgrades tend to take longer.</span></span> <span data-ttu-id="6a2f6-187">Service Fabric čeká krátké době po doména Upgrade a provádí kontroly před zahájením upgradu na další stránku.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-187">Service Fabric waits a short period of time after an Upgrade Domain is completed and performs checks before starting to upgrade the next one.</span></span> <span data-ttu-id="6a2f6-188">Tyto zpoždění povolit zjišťuje problémy, které jsou zavedené službou upgrade před upgradem.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-188">These delays enable detecting issues introduced by the upgrade before the upgrade proceeds.</span></span> <span data-ttu-id="6a2f6-189">Kompromis je přijatelné, protože chybný změny brání by to ovlivnilo příliš mnoho služby najednou.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-189">The tradeoff is acceptable because it prevents bad changes from affecting too much of the service at a time.</span></span>

<span data-ttu-id="6a2f6-190">Příliš málo upgradu domény má mnoho záporné vedlejší účinky – při každé jednotlivé upgradu domény je mimo provoz a upgraduje velkou část celkové kapacity není k dispozici.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-190">Too few Upgrade Domains has many negative side effects – while each individual Upgrade Domain is down and being upgraded a large portion of your overall capacity is unavailable.</span></span> <span data-ttu-id="6a2f6-191">Například pokud máte pouze tři domény upgradu můžete trvá dolů o 1/3 celkové služby nebo kapacity clusteru současně.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-191">For example, if you only have three Upgrade Domains you are taking down about 1/3 of your overall service or cluster capacity at a time.</span></span> <span data-ttu-id="6a2f6-192">Značná část služby najednou s dolů není žádoucí, vzhledem k tomu, že budete muset mít dostatečnou kapacitu ve zbývající části vašeho clusteru tak umožní zpracovávat zatížení.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-192">Having so much of your service down at once isn’t desirable since you have to have enough capacity in the rest of your cluster to handle the workload.</span></span> <span data-ttu-id="6a2f6-193">Zachování této vyrovnávací paměti znamená, že při běžném provozu tyto uzly jsou menší načíst, než by byly jinak.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-193">Maintaining that buffer means that during normal operation those nodes are less loaded than they would be otherwise.</span></span> <span data-ttu-id="6a2f6-194">Tím se zvyšuje náklady na provozování vaší služby.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-194">This increases the cost of running your service.</span></span>

<span data-ttu-id="6a2f6-195">Neexistuje žádné skutečné omezení celkového počtu selhání nebo upgradu domény v prostředí nebo omezení toho, jak se překrývají.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-195">There’s no real limit to the total number of fault or Upgrade Domains in an environment, or constraints on how they overlap.</span></span> <span data-ttu-id="6a2f6-196">Ale nutné dodat, existuje několik běžných vzorů:</span><span class="sxs-lookup"><span data-stu-id="6a2f6-196">That said, there are several common patterns:</span></span>

- <span data-ttu-id="6a2f6-197">Domén selhání a upgradu domény namapovat 1:1</span><span class="sxs-lookup"><span data-stu-id="6a2f6-197">Fault Domains and Upgrade Domains mapped 1:1</span></span>
- <span data-ttu-id="6a2f6-198">Jednu doménu upgradu na uzel (fyzické nebo virtuální instance operačního systému)</span><span class="sxs-lookup"><span data-stu-id="6a2f6-198">One Upgrade Domain per Node (physical or virtual OS instance)</span></span>
- <span data-ttu-id="6a2f6-199">Model "prokládané" nebo "matice", kde domén selhání a upgradu domény formuláři pomocí počítače, které obvykle běží dolů úhlopříčkami matici</span><span class="sxs-lookup"><span data-stu-id="6a2f6-199">A “striped” or “matrix” model where the Fault Domains and Upgrade Domains form a matrix with machines usually running down the diagonals</span></span>

<span data-ttu-id="6a2f6-200"><center>
![Selhání a upgradu domény rozložení][Image4]
</center></span><span class="sxs-lookup"><span data-stu-id="6a2f6-200"><center>
![Fault and Upgrade Domain Layouts][Image4]
</center></span></span>

<span data-ttu-id="6a2f6-201">Není definován že žádný nejvhodnější odpovědět, jaké rozložení byste měli zvolit, každá z nich má některé výhody a nevýhody.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-201">There’s no best answer which layout to choose, each has some pros and cons.</span></span> <span data-ttu-id="6a2f6-202">1FD:1UD model je například jednoduché nastavení.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-202">For example, the 1FD:1UD model is simple to set up.</span></span> <span data-ttu-id="6a2f6-203">1 upgradu domény na uzlu modelu je většina jako osoby, které se používají k.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-203">The 1 Upgrade Domain per Node model is most like what people are used to.</span></span> <span data-ttu-id="6a2f6-204">Během upgradu se aktualizuje každý uzel nezávisle.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-204">During upgrades each node is updated independently.</span></span> <span data-ttu-id="6a2f6-205">Toto je podobná jak malého měla sady počítačů upgradovány ručně v minulosti.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-205">This is similar to how small sets of machines were upgraded manually in the past.</span></span> 

<span data-ttu-id="6a2f6-206">Nejběžnější model je FD/UD matice, kde FDs a UDs vytvoří tabulku a uzly jsou umístěny společně diagonálních spouštění.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-206">The most common model is the FD/UD matrix, where the FDs and UDs form a table and nodes are placed starting along the diagonal.</span></span> <span data-ttu-id="6a2f6-207">Jedná se o model používá ve výchozím nastavení v clusterů Service Fabric v Azure.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-207">This is the model used by default in Service Fabric clusters in Azure.</span></span> <span data-ttu-id="6a2f6-208">U clusterů s uzly mnoho všechno, co skončilo vyhledávání jako vzoru hustých matice výše.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-208">For clusters with many nodes everything ends up looking like the dense matrix pattern above.</span></span>

## <a name="fault-and-upgrade-domain-constraints-and-resulting-behavior"></a><span data-ttu-id="6a2f6-209">Selhání a upgradu domény omezení a výsledné chování</span><span class="sxs-lookup"><span data-stu-id="6a2f6-209">Fault and Upgrade Domain constraints and resulting behavior</span></span>
<span data-ttu-id="6a2f6-210">Správce prostředků clusteru zpracovává pádem si chtít zachovat služby rovnoměrně rozdělen mezi selhání a upgradu domény jako omezení.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-210">The Cluster Resource Manager treats the desire to keep a service balanced across fault and Upgrade Domains as a constraint.</span></span> <span data-ttu-id="6a2f6-211">Můžete najít další informace o omezení v [v tomto článku](service-fabric-cluster-resource-manager-management-integration.md).</span><span class="sxs-lookup"><span data-stu-id="6a2f6-211">You can find out more about constraints in [this article](service-fabric-cluster-resource-manager-management-integration.md).</span></span> <span data-ttu-id="6a2f6-212">Omezení stavu selhání a upgradu domény: "pro oddíl dané služby by nikdy existovat rozdíl *větší než jedna* počet objektů služby (instance bezstavové služby nebo stavové služby repliky) mezi dvě domény."</span><span class="sxs-lookup"><span data-stu-id="6a2f6-212">The Fault and Upgrade Domain constraints state: "For a given service partition there should never be a difference *greater than one* in the number of service objects (stateless service instances or stateful service replicas) between two domains."</span></span> <span data-ttu-id="6a2f6-213">Tím se zabrání určité přesune nebo ujednání, které porušují toto omezení.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-213">This prevents certain moves or arrangements that violate this constraint.</span></span>

<span data-ttu-id="6a2f6-214">Podívejme se na jedním z příkladů.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-214">Let's look at one example.</span></span> <span data-ttu-id="6a2f6-215">Řekněme, že se musí na cluster se šesti uzlů, nakonfigurovaný s pěti domén selhání a pět upgradu domény.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-215">Let's say that we have a cluster with six nodes, configured with five Fault Domains and five Upgrade Domains.</span></span>

|  | <span data-ttu-id="6a2f6-216">FD0</span><span class="sxs-lookup"><span data-stu-id="6a2f6-216">FD0</span></span> | <span data-ttu-id="6a2f6-217">FD1</span><span class="sxs-lookup"><span data-stu-id="6a2f6-217">FD1</span></span> | <span data-ttu-id="6a2f6-218">FD2:</span><span class="sxs-lookup"><span data-stu-id="6a2f6-218">FD2</span></span> | <span data-ttu-id="6a2f6-219">FD3</span><span class="sxs-lookup"><span data-stu-id="6a2f6-219">FD3</span></span> | <span data-ttu-id="6a2f6-220">FD4</span><span class="sxs-lookup"><span data-stu-id="6a2f6-220">FD4</span></span> |
| --- |:---:|:---:|:---:|:---:|:---:|
| <span data-ttu-id="6a2f6-221">**UD0**</span><span class="sxs-lookup"><span data-stu-id="6a2f6-221">**UD0**</span></span> |<span data-ttu-id="6a2f6-222">N1</span><span class="sxs-lookup"><span data-stu-id="6a2f6-222">N1</span></span> | | | | |
| <span data-ttu-id="6a2f6-223">**UD1**</span><span class="sxs-lookup"><span data-stu-id="6a2f6-223">**UD1**</span></span> |<span data-ttu-id="6a2f6-224">N6</span><span class="sxs-lookup"><span data-stu-id="6a2f6-224">N6</span></span> |<span data-ttu-id="6a2f6-225">N2</span><span class="sxs-lookup"><span data-stu-id="6a2f6-225">N2</span></span> | | | |
| <span data-ttu-id="6a2f6-226">**UD2**</span><span class="sxs-lookup"><span data-stu-id="6a2f6-226">**UD2**</span></span> | | |<span data-ttu-id="6a2f6-227">N3</span><span class="sxs-lookup"><span data-stu-id="6a2f6-227">N3</span></span> | | |
| <span data-ttu-id="6a2f6-228">**UD3**</span><span class="sxs-lookup"><span data-stu-id="6a2f6-228">**UD3**</span></span> | | | |<span data-ttu-id="6a2f6-229">N4</span><span class="sxs-lookup"><span data-stu-id="6a2f6-229">N4</span></span> | |
| <span data-ttu-id="6a2f6-230">**UD4**</span><span class="sxs-lookup"><span data-stu-id="6a2f6-230">**UD4**</span></span> | | | | |<span data-ttu-id="6a2f6-231">N5</span><span class="sxs-lookup"><span data-stu-id="6a2f6-231">N5</span></span> |

<span data-ttu-id="6a2f6-232">Nyní Řekněme, že jsme pěti vytvořit službu s TargetReplicaSetSize (nebo bezstavové služby na InstanceCount).</span><span class="sxs-lookup"><span data-stu-id="6a2f6-232">Now let's say that we create a service with a TargetReplicaSetSize (or, for a stateless service an InstanceCount) of five.</span></span> <span data-ttu-id="6a2f6-233">Repliky zobrazovat N1 N5.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-233">The replicas land on N1-N5.</span></span> <span data-ttu-id="6a2f6-234">Ve skutečnosti N6 se nikdy nepoužívá bez ohledu na to, kolik služby, jako to, které vytvoříte.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-234">In fact, N6 is never used no matter how many services like this you create.</span></span> <span data-ttu-id="6a2f6-235">Ale proč?</span><span class="sxs-lookup"><span data-stu-id="6a2f6-235">But why?</span></span> <span data-ttu-id="6a2f6-236">Podívejme se na rozdíl mezi aktuální rozložení a co by mohlo dojít, pokud je zvoleno N6.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-236">Let's look at the difference between the current layout and what would happen if N6 is chosen.</span></span>

<span data-ttu-id="6a2f6-237">Tady je My rozložení a celkového počtu replik na selhání a upgradu domény:</span><span class="sxs-lookup"><span data-stu-id="6a2f6-237">Here's the layout we got and the total number of replicas per Fault and Upgrade Domain:</span></span>

|  | <span data-ttu-id="6a2f6-238">FD0</span><span class="sxs-lookup"><span data-stu-id="6a2f6-238">FD0</span></span> | <span data-ttu-id="6a2f6-239">FD1</span><span class="sxs-lookup"><span data-stu-id="6a2f6-239">FD1</span></span> | <span data-ttu-id="6a2f6-240">FD2:</span><span class="sxs-lookup"><span data-stu-id="6a2f6-240">FD2</span></span> | <span data-ttu-id="6a2f6-241">FD3</span><span class="sxs-lookup"><span data-stu-id="6a2f6-241">FD3</span></span> | <span data-ttu-id="6a2f6-242">FD4</span><span class="sxs-lookup"><span data-stu-id="6a2f6-242">FD4</span></span> | <span data-ttu-id="6a2f6-243">UDTotal</span><span class="sxs-lookup"><span data-stu-id="6a2f6-243">UDTotal</span></span> |
| --- |:---:|:---:|:---:|:---:|:---:|:---:|
| <span data-ttu-id="6a2f6-244">**UD0**</span><span class="sxs-lookup"><span data-stu-id="6a2f6-244">**UD0**</span></span> |<span data-ttu-id="6a2f6-245">R1</span><span class="sxs-lookup"><span data-stu-id="6a2f6-245">R1</span></span> | | | | |<span data-ttu-id="6a2f6-246">1</span><span class="sxs-lookup"><span data-stu-id="6a2f6-246">1</span></span> |
| <span data-ttu-id="6a2f6-247">**UD1**</span><span class="sxs-lookup"><span data-stu-id="6a2f6-247">**UD1**</span></span> | |<span data-ttu-id="6a2f6-248">R2</span><span class="sxs-lookup"><span data-stu-id="6a2f6-248">R2</span></span> | | | |<span data-ttu-id="6a2f6-249">1</span><span class="sxs-lookup"><span data-stu-id="6a2f6-249">1</span></span> |
| <span data-ttu-id="6a2f6-250">**UD2**</span><span class="sxs-lookup"><span data-stu-id="6a2f6-250">**UD2**</span></span> | | |<span data-ttu-id="6a2f6-251">R3</span><span class="sxs-lookup"><span data-stu-id="6a2f6-251">R3</span></span> | | |<span data-ttu-id="6a2f6-252">1</span><span class="sxs-lookup"><span data-stu-id="6a2f6-252">1</span></span> |
| <span data-ttu-id="6a2f6-253">**UD3**</span><span class="sxs-lookup"><span data-stu-id="6a2f6-253">**UD3**</span></span> | | | |<span data-ttu-id="6a2f6-254">R4</span><span class="sxs-lookup"><span data-stu-id="6a2f6-254">R4</span></span> | |<span data-ttu-id="6a2f6-255">1</span><span class="sxs-lookup"><span data-stu-id="6a2f6-255">1</span></span> |
| <span data-ttu-id="6a2f6-256">**UD4**</span><span class="sxs-lookup"><span data-stu-id="6a2f6-256">**UD4**</span></span> | | | | |<span data-ttu-id="6a2f6-257">R5</span><span class="sxs-lookup"><span data-stu-id="6a2f6-257">R5</span></span> |<span data-ttu-id="6a2f6-258">1</span><span class="sxs-lookup"><span data-stu-id="6a2f6-258">1</span></span> |
| <span data-ttu-id="6a2f6-259">**FDTotal**</span><span class="sxs-lookup"><span data-stu-id="6a2f6-259">**FDTotal**</span></span> |<span data-ttu-id="6a2f6-260">1</span><span class="sxs-lookup"><span data-stu-id="6a2f6-260">1</span></span> |<span data-ttu-id="6a2f6-261">1</span><span class="sxs-lookup"><span data-stu-id="6a2f6-261">1</span></span> |<span data-ttu-id="6a2f6-262">1</span><span class="sxs-lookup"><span data-stu-id="6a2f6-262">1</span></span> |<span data-ttu-id="6a2f6-263">1</span><span class="sxs-lookup"><span data-stu-id="6a2f6-263">1</span></span> |<span data-ttu-id="6a2f6-264">1</span><span class="sxs-lookup"><span data-stu-id="6a2f6-264">1</span></span> |- |

<span data-ttu-id="6a2f6-265">Toto rozložení jsou rovnoměrně z hlediska uzlů na doméně selhání a upgradu domény.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-265">This layout is balanced in terms of nodes per Fault Domain and Upgrade Domain.</span></span> <span data-ttu-id="6a2f6-266">Toto pravidlo jsou rovnoměrně také z hlediska počtu replik na selhání a upgradu domény.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-266">It is also balanced in terms of the number of replicas per Fault and Upgrade Domain.</span></span> <span data-ttu-id="6a2f6-267">Každá doména má stejný počet uzlů a stejný počet replik.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-267">Each domain has the same number of nodes and the same number of replicas.</span></span>

<span data-ttu-id="6a2f6-268">Teď se podíváme, co by se stalo jsme použití N6 místo N2.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-268">Now, let's look at what would happen if we'd used N6 instead of N2.</span></span> <span data-ttu-id="6a2f6-269">Jak by repliky distribuována pak?</span><span class="sxs-lookup"><span data-stu-id="6a2f6-269">How would the replicas be distributed then?</span></span>

|  | <span data-ttu-id="6a2f6-270">FD0</span><span class="sxs-lookup"><span data-stu-id="6a2f6-270">FD0</span></span> | <span data-ttu-id="6a2f6-271">FD1</span><span class="sxs-lookup"><span data-stu-id="6a2f6-271">FD1</span></span> | <span data-ttu-id="6a2f6-272">FD2:</span><span class="sxs-lookup"><span data-stu-id="6a2f6-272">FD2</span></span> | <span data-ttu-id="6a2f6-273">FD3</span><span class="sxs-lookup"><span data-stu-id="6a2f6-273">FD3</span></span> | <span data-ttu-id="6a2f6-274">FD4</span><span class="sxs-lookup"><span data-stu-id="6a2f6-274">FD4</span></span> | <span data-ttu-id="6a2f6-275">UDTotal</span><span class="sxs-lookup"><span data-stu-id="6a2f6-275">UDTotal</span></span> |
| --- |:---:|:---:|:---:|:---:|:---:|:---:|
| <span data-ttu-id="6a2f6-276">**UD0**</span><span class="sxs-lookup"><span data-stu-id="6a2f6-276">**UD0**</span></span> |<span data-ttu-id="6a2f6-277">R1</span><span class="sxs-lookup"><span data-stu-id="6a2f6-277">R1</span></span> | | | | |<span data-ttu-id="6a2f6-278">1</span><span class="sxs-lookup"><span data-stu-id="6a2f6-278">1</span></span> |
| <span data-ttu-id="6a2f6-279">**UD1**</span><span class="sxs-lookup"><span data-stu-id="6a2f6-279">**UD1**</span></span> |<span data-ttu-id="6a2f6-280">R5</span><span class="sxs-lookup"><span data-stu-id="6a2f6-280">R5</span></span> | | | | |<span data-ttu-id="6a2f6-281">1</span><span class="sxs-lookup"><span data-stu-id="6a2f6-281">1</span></span> |
| <span data-ttu-id="6a2f6-282">**UD2**</span><span class="sxs-lookup"><span data-stu-id="6a2f6-282">**UD2**</span></span> | | |<span data-ttu-id="6a2f6-283">R2</span><span class="sxs-lookup"><span data-stu-id="6a2f6-283">R2</span></span> | | |<span data-ttu-id="6a2f6-284">1</span><span class="sxs-lookup"><span data-stu-id="6a2f6-284">1</span></span> |
| <span data-ttu-id="6a2f6-285">**UD3**</span><span class="sxs-lookup"><span data-stu-id="6a2f6-285">**UD3**</span></span> | | | |<span data-ttu-id="6a2f6-286">R3</span><span class="sxs-lookup"><span data-stu-id="6a2f6-286">R3</span></span> | |<span data-ttu-id="6a2f6-287">1</span><span class="sxs-lookup"><span data-stu-id="6a2f6-287">1</span></span> |
| <span data-ttu-id="6a2f6-288">**UD4**</span><span class="sxs-lookup"><span data-stu-id="6a2f6-288">**UD4**</span></span> | | | | |<span data-ttu-id="6a2f6-289">R4</span><span class="sxs-lookup"><span data-stu-id="6a2f6-289">R4</span></span> |<span data-ttu-id="6a2f6-290">1</span><span class="sxs-lookup"><span data-stu-id="6a2f6-290">1</span></span> |
| <span data-ttu-id="6a2f6-291">**FDTotal**</span><span class="sxs-lookup"><span data-stu-id="6a2f6-291">**FDTotal**</span></span> |<span data-ttu-id="6a2f6-292">2</span><span class="sxs-lookup"><span data-stu-id="6a2f6-292">2</span></span> |<span data-ttu-id="6a2f6-293">0</span><span class="sxs-lookup"><span data-stu-id="6a2f6-293">0</span></span> |<span data-ttu-id="6a2f6-294">1</span><span class="sxs-lookup"><span data-stu-id="6a2f6-294">1</span></span> |<span data-ttu-id="6a2f6-295">1</span><span class="sxs-lookup"><span data-stu-id="6a2f6-295">1</span></span> |<span data-ttu-id="6a2f6-296">1</span><span class="sxs-lookup"><span data-stu-id="6a2f6-296">1</span></span> |- |

<span data-ttu-id="6a2f6-297">Toto rozložení porušuje naše definice omezení domény selhání.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-297">This layout violates our definition for the Fault Domain constraint.</span></span> <span data-ttu-id="6a2f6-298">FD0 má dvě repliky, zatímco FD1 obsahuje nula, a tím rozdíl mezi FD0 a FD1 celkem dva.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-298">FD0 has two replicas, while FD1 has zero, making the difference between FD0 and FD1 a total of two.</span></span> <span data-ttu-id="6a2f6-299">Správce prostředků clusteru neumožňuje toto uspořádání.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-299">The Cluster Resource Manager does not allow this arrangement.</span></span> <span data-ttu-id="6a2f6-300">Podobně pokud jsme zachyceny N2 a N6 (namísto N1 a N2) nám se získat:</span><span class="sxs-lookup"><span data-stu-id="6a2f6-300">Similarly if we picked N2 and N6 (instead of N1 and N2) we'd get:</span></span>

|  | <span data-ttu-id="6a2f6-301">FD0</span><span class="sxs-lookup"><span data-stu-id="6a2f6-301">FD0</span></span> | <span data-ttu-id="6a2f6-302">FD1</span><span class="sxs-lookup"><span data-stu-id="6a2f6-302">FD1</span></span> | <span data-ttu-id="6a2f6-303">FD2:</span><span class="sxs-lookup"><span data-stu-id="6a2f6-303">FD2</span></span> | <span data-ttu-id="6a2f6-304">FD3</span><span class="sxs-lookup"><span data-stu-id="6a2f6-304">FD3</span></span> | <span data-ttu-id="6a2f6-305">FD4</span><span class="sxs-lookup"><span data-stu-id="6a2f6-305">FD4</span></span> | <span data-ttu-id="6a2f6-306">UDTotal</span><span class="sxs-lookup"><span data-stu-id="6a2f6-306">UDTotal</span></span> |
| --- |:---:|:---:|:---:|:---:|:---:|:---:|
| <span data-ttu-id="6a2f6-307">**UD0**</span><span class="sxs-lookup"><span data-stu-id="6a2f6-307">**UD0**</span></span> | | | | | |<span data-ttu-id="6a2f6-308">0</span><span class="sxs-lookup"><span data-stu-id="6a2f6-308">0</span></span> |
| <span data-ttu-id="6a2f6-309">**UD1**</span><span class="sxs-lookup"><span data-stu-id="6a2f6-309">**UD1**</span></span> |<span data-ttu-id="6a2f6-310">R5</span><span class="sxs-lookup"><span data-stu-id="6a2f6-310">R5</span></span> |<span data-ttu-id="6a2f6-311">R1</span><span class="sxs-lookup"><span data-stu-id="6a2f6-311">R1</span></span> | | | |<span data-ttu-id="6a2f6-312">2</span><span class="sxs-lookup"><span data-stu-id="6a2f6-312">2</span></span> |
| <span data-ttu-id="6a2f6-313">**UD2**</span><span class="sxs-lookup"><span data-stu-id="6a2f6-313">**UD2**</span></span> | | |<span data-ttu-id="6a2f6-314">R2</span><span class="sxs-lookup"><span data-stu-id="6a2f6-314">R2</span></span> | | |<span data-ttu-id="6a2f6-315">1</span><span class="sxs-lookup"><span data-stu-id="6a2f6-315">1</span></span> |
| <span data-ttu-id="6a2f6-316">**UD3**</span><span class="sxs-lookup"><span data-stu-id="6a2f6-316">**UD3**</span></span> | | | |<span data-ttu-id="6a2f6-317">R3</span><span class="sxs-lookup"><span data-stu-id="6a2f6-317">R3</span></span> | |<span data-ttu-id="6a2f6-318">1</span><span class="sxs-lookup"><span data-stu-id="6a2f6-318">1</span></span> |
| <span data-ttu-id="6a2f6-319">**UD4**</span><span class="sxs-lookup"><span data-stu-id="6a2f6-319">**UD4**</span></span> | | | | |<span data-ttu-id="6a2f6-320">R4</span><span class="sxs-lookup"><span data-stu-id="6a2f6-320">R4</span></span> |<span data-ttu-id="6a2f6-321">1</span><span class="sxs-lookup"><span data-stu-id="6a2f6-321">1</span></span> |
| <span data-ttu-id="6a2f6-322">**FDTotal**</span><span class="sxs-lookup"><span data-stu-id="6a2f6-322">**FDTotal**</span></span> |<span data-ttu-id="6a2f6-323">1</span><span class="sxs-lookup"><span data-stu-id="6a2f6-323">1</span></span> |<span data-ttu-id="6a2f6-324">1</span><span class="sxs-lookup"><span data-stu-id="6a2f6-324">1</span></span> |<span data-ttu-id="6a2f6-325">1</span><span class="sxs-lookup"><span data-stu-id="6a2f6-325">1</span></span> |<span data-ttu-id="6a2f6-326">1</span><span class="sxs-lookup"><span data-stu-id="6a2f6-326">1</span></span> |<span data-ttu-id="6a2f6-327">1</span><span class="sxs-lookup"><span data-stu-id="6a2f6-327">1</span></span> |- |

<span data-ttu-id="6a2f6-328">Toto rozložení jsou rovnoměrně z hlediska domén selhání.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-328">This layout is balanced in terms of Fault Domains.</span></span> <span data-ttu-id="6a2f6-329">Ale nyní je porušuje omezení upgradu domény.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-329">However, now it's violating the Upgrade Domain constraint.</span></span> <span data-ttu-id="6a2f6-330">To je proto UD0 má nulové repliky, zatímco UD1 má dva.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-330">This is because UD0 has zero replicas while UD1 has two.</span></span> <span data-ttu-id="6a2f6-331">Toto rozložení proto je neplatná a nebude být zachyceny pomocí Správce prostředků clusteru.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-331">Therefore, this layout is also invalid and won't be picked by the Cluster Resource Manager.</span></span> 

## <a name="configuring-fault-and-upgrade-domains"></a><span data-ttu-id="6a2f6-332">Konfigurace selhání a upgradu domény</span><span class="sxs-lookup"><span data-stu-id="6a2f6-332">Configuring fault and Upgrade Domains</span></span>
<span data-ttu-id="6a2f6-333">Definování domén selhání a upgradu domény se provádí automaticky v Azure hostovaná nasazení Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-333">Defining Fault Domains and Upgrade Domains is done automatically in Azure hosted Service Fabric deployments.</span></span> <span data-ttu-id="6a2f6-334">Service Fabric převezme a používá informace o prostředí z Azure.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-334">Service Fabric picks up and uses the environment information from Azure.</span></span>

<span data-ttu-id="6a2f6-335">Pokud vytváříte vlastní cluster (nebo chcete spustit konkrétní topologie vývojem), je informace o doméně selhání a upgradu domény můžete zadat sami.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-335">If you’re creating your own cluster (or want to run a particular topology in development), you can provide the Fault Domain and Upgrade Domain information yourself.</span></span> <span data-ttu-id="6a2f6-336">V tomto příkladu jsme definovali místní vývojový cluster devět uzlu, který zahrnuje tři "datových centrech" (každý má tři stojany).</span><span class="sxs-lookup"><span data-stu-id="6a2f6-336">In this example, we define a nine node local development cluster that spans three “datacenters” (each with three racks).</span></span> <span data-ttu-id="6a2f6-337">Tento cluster má také tři domény upgradu rozdělená mezi tyto tři datových centrech.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-337">This cluster also has three Upgrade Domains striped across those three datacenters.</span></span> <span data-ttu-id="6a2f6-338">Zde je příklad konfigurace:</span><span class="sxs-lookup"><span data-stu-id="6a2f6-338">An example of the configuration is below:</span></span> 

<span data-ttu-id="6a2f6-339">ClusterManifest.xml</span><span class="sxs-lookup"><span data-stu-id="6a2f6-339">ClusterManifest.xml</span></span>

```xml
  <Infrastructure>
    <!-- IsScaleMin indicates that this cluster runs on one-box /one single server -->
    <WindowsServer IsScaleMin="true">
      <NodeList>
        <Node NodeName="Node01" IPAddressOrFQDN="localhost" NodeTypeRef="NodeType01" FaultDomain="fd:/DC01/Rack01" UpgradeDomain="UpgradeDomain1" IsSeedNode="true" />
        <Node NodeName="Node02" IPAddressOrFQDN="localhost" NodeTypeRef="NodeType02" FaultDomain="fd:/DC01/Rack02" UpgradeDomain="UpgradeDomain2" IsSeedNode="true" />
        <Node NodeName="Node03" IPAddressOrFQDN="localhost" NodeTypeRef="NodeType03" FaultDomain="fd:/DC01/Rack03" UpgradeDomain="UpgradeDomain3" IsSeedNode="true" />
        <Node NodeName="Node04" IPAddressOrFQDN="localhost" NodeTypeRef="NodeType04" FaultDomain="fd:/DC02/Rack01" UpgradeDomain="UpgradeDomain1" IsSeedNode="true" />
        <Node NodeName="Node05" IPAddressOrFQDN="localhost" NodeTypeRef="NodeType05" FaultDomain="fd:/DC02/Rack02" UpgradeDomain="UpgradeDomain2" IsSeedNode="true" />
        <Node NodeName="Node06" IPAddressOrFQDN="localhost" NodeTypeRef="NodeType06" FaultDomain="fd:/DC02/Rack03" UpgradeDomain="UpgradeDomain3" IsSeedNode="true" />
        <Node NodeName="Node07" IPAddressOrFQDN="localhost" NodeTypeRef="NodeType07" FaultDomain="fd:/DC03/Rack01" UpgradeDomain="UpgradeDomain1" IsSeedNode="true" />
        <Node NodeName="Node08" IPAddressOrFQDN="localhost" NodeTypeRef="NodeType08" FaultDomain="fd:/DC03/Rack02" UpgradeDomain="UpgradeDomain2" IsSeedNode="true" />
        <Node NodeName="Node09" IPAddressOrFQDN="localhost" NodeTypeRef="NodeType09" FaultDomain="fd:/DC03/Rack03" UpgradeDomain="UpgradeDomain3" IsSeedNode="true" />
      </NodeList>
    </WindowsServer>
  </Infrastructure>
```

<span data-ttu-id="6a2f6-340">pomocí souboru ClusterConfig.json pro samostatné nasazení</span><span class="sxs-lookup"><span data-stu-id="6a2f6-340">via ClusterConfig.json for Standalone deployments</span></span>

```json
"nodes": [
  {
    "nodeName": "vm1",
    "iPAddress": "localhost",
    "nodeTypeRef": "NodeType0",
    "faultDomain": "fd:/dc1/r0",
    "upgradeDomain": "UD1"
  },
  {
    "nodeName": "vm2",
    "iPAddress": "localhost",
    "nodeTypeRef": "NodeType0",
    "faultDomain": "fd:/dc1/r0",
    "upgradeDomain": "UD2"
  },
  {
    "nodeName": "vm3",
    "iPAddress": "localhost",
    "nodeTypeRef": "NodeType0",
    "faultDomain": "fd:/dc1/r0",
    "upgradeDomain": "UD3"
  },
  {
    "nodeName": "vm4",
    "iPAddress": "localhost",
    "nodeTypeRef": "NodeType0",
    "faultDomain": "fd:/dc2/r0",
    "upgradeDomain": "UD1"
  },
  {
    "nodeName": "vm5",
    "iPAddress": "localhost",
    "nodeTypeRef": "NodeType0",
    "faultDomain": "fd:/dc2/r0",
    "upgradeDomain": "UD2"
  },
  {
    "nodeName": "vm6",
    "iPAddress": "localhost",
    "nodeTypeRef": "NodeType0",
    "faultDomain": "fd:/dc2/r0",
    "upgradeDomain": "UD3"
  },
  {
    "nodeName": "vm7",
    "iPAddress": "localhost",
    "nodeTypeRef": "NodeType0",
    "faultDomain": "fd:/dc3/r0",
    "upgradeDomain": "UD1"
  },
  {
    "nodeName": "vm8",
    "iPAddress": "localhost",
    "nodeTypeRef": "NodeType0",
    "faultDomain": "fd:/dc3/r0",
    "upgradeDomain": "UD2"
  },
  {
    "nodeName": "vm9",
    "iPAddress": "localhost",
    "nodeTypeRef": "NodeType0",
    "faultDomain": "fd:/dc3/r0",
    "upgradeDomain": "UD3"
  }
],
```

> [!NOTE]
> <span data-ttu-id="6a2f6-341">Při definování clusterů pomocí Azure Resource Manager, jsou přiřazeny domén selhání a upgradu domény Azure.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-341">When defining clusters via Azure Resource Manager, Fault Domains and Upgrade Domains are assigned by Azure.</span></span> <span data-ttu-id="6a2f6-342">Definice typů uzlů a sady škálování virtuálního počítače do šablony Azure Resource Manager tedy nezahrnuje doména selhání nebo informace o upgradu domény.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-342">Therefore, the definition of your Node Types and Virtual Machine Scale Sets in your Azure Resource Manager template does not include Fault Domain or Upgrade Domain information.</span></span>
>

## <a name="node-properties-and-placement-constraints"></a><span data-ttu-id="6a2f6-343">Vlastnosti uzlu a omezení umístění</span><span class="sxs-lookup"><span data-stu-id="6a2f6-343">Node properties and placement constraints</span></span>
<span data-ttu-id="6a2f6-344">V některých případech (v faktu, ve většině případů) budete chtít zajistit, že určité úlohy spustit pouze v určitých typů uzlů v clusteru.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-344">Sometimes (in fact, most of the time) you’re going to want to ensure that certain workloads run only on certain types of nodes in the cluster.</span></span> <span data-ttu-id="6a2f6-345">Pro některé zatížení může například vyžadovat grafickými procesory nebo SSD a jiné nikoli.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-345">For example, some workload may require GPUs or SSDs while others may not.</span></span> <span data-ttu-id="6a2f6-346">Skvělé příkladem cílení na hardware pro konkrétní úlohy je téměř každé n vrstvá architektura odhlašování došlo.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-346">A great example of targeting hardware to particular workloads is almost every n-tier architecture out there.</span></span> <span data-ttu-id="6a2f6-347">Některé počítače sloužit jako front-endu nebo obsluhující straně aplikace API a jsou viditelné na klienty nebo internet.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-347">Certain machines serve as the front end or API serving side of the application and are exposed to the clients or the internet.</span></span> <span data-ttu-id="6a2f6-348">Různé počítače, často se různé hardwarové prostředky, zpracování pracovní vrstev výpočtů nebo úložiště.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-348">Different machines, often with different hardware resources, handle the work of the compute or storage layers.</span></span> <span data-ttu-id="6a2f6-349">Obvykle se jedná o _není_ přímo vystavený pro klienty nebo Internetu.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-349">These are usually _not_ directly exposed to clients or the internet.</span></span> <span data-ttu-id="6a2f6-350">Service Fabric očekává, že existují případy, kdy je potřeba spustit na konkrétní hardwarové konfigurace konkrétní úlohy.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-350">Service Fabric expects that there are cases where particular workloads need to run on particular hardware configurations.</span></span> <span data-ttu-id="6a2f6-351">Například:</span><span class="sxs-lookup"><span data-stu-id="6a2f6-351">For example:</span></span>

* <span data-ttu-id="6a2f6-352">stávající vícevrstvé aplikace byla "zrušeno a zapuštěno" do prostředí Service Fabric</span><span class="sxs-lookup"><span data-stu-id="6a2f6-352">an existing n-tier application has been “lifted and shifted” into a Service Fabric environment</span></span>
* <span data-ttu-id="6a2f6-353">zatížení chce, aby běžela na konkrétní hardware pro výkon, škálování nebo z důvodu izolace zabezpečení</span><span class="sxs-lookup"><span data-stu-id="6a2f6-353">a workload wants to run on specific hardware for performance, scale, or security isolation reasons</span></span>
* <span data-ttu-id="6a2f6-354">Úloha by měla být izolované od ostatních úloh pro zásady nebo prostředek spotřebu důvody</span><span class="sxs-lookup"><span data-stu-id="6a2f6-354">A workload should be isolated from other workloads for policy or resource consumption reasons</span></span>

<span data-ttu-id="6a2f6-355">Pro podporu těchto nastavení neovlivní konfigurace, Service Fabric má první třídy představu o značky, které lze použít na uzly.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-355">To support these sorts of configurations, Service Fabric has a first class notion of tags that can be applied to nodes.</span></span> <span data-ttu-id="6a2f6-356">Tyto značky se nazývají **vlastnosti uzlu**.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-356">These tags are called **node properties**.</span></span> <span data-ttu-id="6a2f6-357">**Omezení umístění** jsou příkazy, které jsou připojené k jednotlivé služby, které vyberte jeden nebo více vlastností uzlu.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-357">**Placement constraints** are the statements attached to individual services that select for one or more node properties.</span></span> <span data-ttu-id="6a2f6-358">Omezení umístění definovat, které by měly běžet služby.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-358">Placement constraints define where services should run.</span></span> <span data-ttu-id="6a2f6-359">Sadu omezení je rozšiřitelný – může fungovat všechny dvojice klíč/hodnota.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-359">The set of constraints is extensible - any key/value pair can work.</span></span> 

<span data-ttu-id="6a2f6-360"><center>
![Různé rozložení zatížení clusteru][Image5]
</center></span><span class="sxs-lookup"><span data-stu-id="6a2f6-360"><center>
![Cluster Layout Different Workloads][Image5]
</center></span></span>

### <a name="built-in-node-properties"></a><span data-ttu-id="6a2f6-361">Součástí vlastnosti uzlu</span><span class="sxs-lookup"><span data-stu-id="6a2f6-361">Built in node properties</span></span>
<span data-ttu-id="6a2f6-362">Service Fabric definuje některé výchozí uzel vlastnosti, které lze použít automaticky bez nutnosti definovat uživatele.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-362">Service Fabric defines some default node properties that can be used automatically without the user having to define them.</span></span> <span data-ttu-id="6a2f6-363">Výchozí vlastnosti definované v každém uzlu jsou **NodeType** a **NodeName**.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-363">The default properties defined at each node are the **NodeType** and the **NodeName**.</span></span> <span data-ttu-id="6a2f6-364">Můžete napsat tak například omezení umístění jako `"(NodeType == NodeType03)"`.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-364">So for example you could write a placement constraint as `"(NodeType == NodeType03)"`.</span></span> <span data-ttu-id="6a2f6-365">Obecně našli jsme, že NodeType bude jeden z nejvíc běžně používá vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-365">Generally we have found NodeType to be one of the most commonly used properties.</span></span> <span data-ttu-id="6a2f6-366">Je vhodné vzhledem k tomu, že odpovídá 1:1 s typem počítače.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-366">It is useful since it corresponds 1:1 with a type of a machine.</span></span> <span data-ttu-id="6a2f6-367">Každý typ počítače odpovídá typu úlohy v tradiční n vrstvá aplikaci.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-367">Each type of machine corresponds to a type of workload in a traditional n-tier application.</span></span>

<span data-ttu-id="6a2f6-368"><center>
![Omezení umístění a vlastnosti uzlu][Image6]
</center></span><span class="sxs-lookup"><span data-stu-id="6a2f6-368"><center>
![Placement Constraints and Node Properties][Image6]
</center></span></span>

## <a name="placement-constraint-and-node-property-syntax"></a><span data-ttu-id="6a2f6-369">Omezení umístění a syntaxe vlastnosti uzlu</span><span class="sxs-lookup"><span data-stu-id="6a2f6-369">Placement Constraint and Node Property Syntax</span></span> 
<span data-ttu-id="6a2f6-370">Hodnota zadaná ve vlastnosti uzlu může být řetězec, bool, nebo podepsané dlouho.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-370">The value specified in the node property can be a string, bool, or signed long.</span></span> <span data-ttu-id="6a2f6-371">Příkaz na službu se označuje jako umístění *omezení* vzhledem k tomu, že se omezí, kde se služba může běžet v clusteru.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-371">The statement at the service is called a placement *constraint* since it constrains where the service can run in the cluster.</span></span> <span data-ttu-id="6a2f6-372">Omezení může být jakékoli Boolean příkaz, který funguje ve vlastnostech jiného uzlu v clusteru.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-372">The constraint can be any Boolean statement that operates on the different node properties in the cluster.</span></span> <span data-ttu-id="6a2f6-373">Jsou platné selektory v těchto příkazech logická hodnota:</span><span class="sxs-lookup"><span data-stu-id="6a2f6-373">The valid selectors in these boolean statements are:</span></span>

1) <span data-ttu-id="6a2f6-374">Podmíněné kontroly pro vytvoření konkrétní příkazy</span><span class="sxs-lookup"><span data-stu-id="6a2f6-374">conditional checks for creating particular statements</span></span>

| <span data-ttu-id="6a2f6-375">Příkaz</span><span class="sxs-lookup"><span data-stu-id="6a2f6-375">Statement</span></span> | <span data-ttu-id="6a2f6-376">Syntaxe</span><span class="sxs-lookup"><span data-stu-id="6a2f6-376">Syntax</span></span> |
| --- |:---:|
| <span data-ttu-id="6a2f6-377">"rovno"</span><span class="sxs-lookup"><span data-stu-id="6a2f6-377">"equal to"</span></span> | <span data-ttu-id="6a2f6-378">"=="</span><span class="sxs-lookup"><span data-stu-id="6a2f6-378">"=="</span></span> |
| <span data-ttu-id="6a2f6-379">"není rovno"</span><span class="sxs-lookup"><span data-stu-id="6a2f6-379">"not equal to"</span></span> | <span data-ttu-id="6a2f6-380">"!="</span><span class="sxs-lookup"><span data-stu-id="6a2f6-380">"!="</span></span> |
| <span data-ttu-id="6a2f6-381">"větší než"</span><span class="sxs-lookup"><span data-stu-id="6a2f6-381">"greater than"</span></span> | <span data-ttu-id="6a2f6-382">">"</span><span class="sxs-lookup"><span data-stu-id="6a2f6-382">">"</span></span> |
| <span data-ttu-id="6a2f6-383">"větší než nebo rovno"</span><span class="sxs-lookup"><span data-stu-id="6a2f6-383">"greater than or equal to"</span></span> | <span data-ttu-id="6a2f6-384">">="</span><span class="sxs-lookup"><span data-stu-id="6a2f6-384">">="</span></span> |
| <span data-ttu-id="6a2f6-385">"menší než"</span><span class="sxs-lookup"><span data-stu-id="6a2f6-385">"less than"</span></span> | <span data-ttu-id="6a2f6-386">"<"</span><span class="sxs-lookup"><span data-stu-id="6a2f6-386">"<"</span></span> |
| <span data-ttu-id="6a2f6-387">"menší než nebo rovno"</span><span class="sxs-lookup"><span data-stu-id="6a2f6-387">"less than or equal to"</span></span> | <span data-ttu-id="6a2f6-388">"<="</span><span class="sxs-lookup"><span data-stu-id="6a2f6-388">"<="</span></span> |

2) <span data-ttu-id="6a2f6-389">Logická hodnota příkazy pro seskupování a logické operace</span><span class="sxs-lookup"><span data-stu-id="6a2f6-389">boolean statements for grouping and logical operations</span></span>

| <span data-ttu-id="6a2f6-390">Příkaz</span><span class="sxs-lookup"><span data-stu-id="6a2f6-390">Statement</span></span> | <span data-ttu-id="6a2f6-391">Syntaxe</span><span class="sxs-lookup"><span data-stu-id="6a2f6-391">Syntax</span></span> |
| --- |:---:|
| <span data-ttu-id="6a2f6-392">"a"</span><span class="sxs-lookup"><span data-stu-id="6a2f6-392">"and"</span></span> | <span data-ttu-id="6a2f6-393">"&&"</span><span class="sxs-lookup"><span data-stu-id="6a2f6-393">"&&"</span></span> |
| <span data-ttu-id="6a2f6-394">"nebo"</span><span class="sxs-lookup"><span data-stu-id="6a2f6-394">"or"</span></span> | <span data-ttu-id="6a2f6-395">"&#124;&#124;"</span><span class="sxs-lookup"><span data-stu-id="6a2f6-395">"&#124;&#124;"</span></span> |
| <span data-ttu-id="6a2f6-396">"Ne"</span><span class="sxs-lookup"><span data-stu-id="6a2f6-396">"not"</span></span> | <span data-ttu-id="6a2f6-397">"!"</span><span class="sxs-lookup"><span data-stu-id="6a2f6-397">"!"</span></span> |
| <span data-ttu-id="6a2f6-398">"skupiny jako jediný příkaz"</span><span class="sxs-lookup"><span data-stu-id="6a2f6-398">"group as single statement"</span></span> | <span data-ttu-id="6a2f6-399">"()"</span><span class="sxs-lookup"><span data-stu-id="6a2f6-399">"()"</span></span> |

<span data-ttu-id="6a2f6-400">Zde jsou některé příklady příkazů základní omezení.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-400">Here are some examples of basic constraint statements.</span></span>

  * `"Value >= 5"`
  * `"NodeColor != green"`
  * `"((OneProperty < 100) || ((AnotherProperty == false) && (OneProperty >= 100)))"`

<span data-ttu-id="6a2f6-401">Službu umístit na něm může mít pouze uzly, které vyhodnotí příkaz celkové omezení umístění na hodnotu "True".</span><span class="sxs-lookup"><span data-stu-id="6a2f6-401">Only nodes where the overall placement constraint statement evaluates to “True” can have the service placed on it.</span></span> <span data-ttu-id="6a2f6-402">Uzly, které nemají vlastnost definovanou se neshodují žádné omezení umístění obsahující tuto vlastnost.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-402">Nodes that do not have a property defined do not match any placement constraint containing that property.</span></span>

<span data-ttu-id="6a2f6-403">Řekněme, že následující vlastnosti uzlu byly definovány pro daný uzel typu:</span><span class="sxs-lookup"><span data-stu-id="6a2f6-403">Let’s say that the following node properties were defined for a given node type:</span></span>

<span data-ttu-id="6a2f6-404">ClusterManifest.xml</span><span class="sxs-lookup"><span data-stu-id="6a2f6-404">ClusterManifest.xml</span></span>

```xml
    <NodeType Name="NodeType01">
      <PlacementProperties>
        <Property Name="HasSSD" Value="true"/>
        <Property Name="NodeColor" Value="green"/>
        <Property Name="SomeProperty" Value="5"/>
      </PlacementProperties>
    </NodeType>
```

<span data-ttu-id="6a2f6-405">pomocí souboru ClusterConfig.json pro samostatné nasazení nebo Template.json pro Azure hostované clustery.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-405">via ClusterConfig.json for Standalone deployments or Template.json for Azure hosted clusters.</span></span> 

> [!NOTE]
> <span data-ttu-id="6a2f6-406">V šabloně Azure Resource Manager je obvykle parametry typ uzlu.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-406">In your Azure Resource Manager template the node type is usually parameterized.</span></span> <span data-ttu-id="6a2f6-407">Může vypadat třeba "[parameters('vmNodeType1Name')]" místo "NodeType01".</span><span class="sxs-lookup"><span data-stu-id="6a2f6-407">It would look like "[parameters('vmNodeType1Name')]" rather than "NodeType01".</span></span>
>

```json
"nodeTypes": [
    {
        "name": "NodeType01",
        "placementProperties": {
            "HasSSD": "true",
            "NodeColor": "green",
            "SomeProperty": "5"
        },
    }
],
```

<span data-ttu-id="6a2f6-408">Můžete vytvořit umístění služby *omezení* pro službu, jako následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="6a2f6-408">You can create service placement *constraints* for a service like as follows:</span></span>

<span data-ttu-id="6a2f6-409">C#</span><span class="sxs-lookup"><span data-stu-id="6a2f6-409">C#</span></span>

```csharp
FabricClient fabricClient = new FabricClient();
StatefulServiceDescription serviceDescription = new StatefulServiceDescription();
serviceDescription.PlacementConstraints = "(HasSSD == true && SomeProperty >= 4)";
// add other required servicedescription fields
//...
await fabricClient.ServiceManager.CreateServiceAsync(serviceDescription);
```

<span data-ttu-id="6a2f6-410">Prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="6a2f6-410">Powershell:</span></span>

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceType -Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -PlacementConstraint "HasSSD == true && SomeProperty >= 4"
```

<span data-ttu-id="6a2f6-411">Pokud jsou všechny uzly NodeType01 platné, můžete také vybrat daný typ uzlu s omezením v atributu "(NodeType == NodeType01)".</span><span class="sxs-lookup"><span data-stu-id="6a2f6-411">If all nodes of NodeType01 are valid, you can also select that node type with the constraint "(NodeType == NodeType01)".</span></span>

<span data-ttu-id="6a2f6-412">Jedním z nástrojů věcí o omezení umístění služby je, aby bylo možné aktualizovat dynamicky za běhu.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-412">One of the cool things about a service’s placement constraints is that they can be updated dynamically during runtime.</span></span> <span data-ttu-id="6a2f6-413">Takže pokud budete potřebovat, abyste můžete pohyb služby v clusteru, přidávat a odebírat požadavky atd. Service Fabric postará zajistit, že službu zůstává nahoru a k dispozici i v případě, že jsou vytvářeny těchto typů změn.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-413">So if you need to, you can move a service around in the cluster, add and remove requirements, etc. Service Fabric takes care of ensuring that the service stays up and available even when these types of changes are made.</span></span>

<span data-ttu-id="6a2f6-414">C#:</span><span class="sxs-lookup"><span data-stu-id="6a2f6-414">C#:</span></span>

```csharp
StatefulServiceUpdateDescription updateDescription = new StatefulServiceUpdateDescription();
updateDescription.PlacementConstraints = "NodeType == NodeType01";
await fabricClient.ServiceManager.UpdateServiceAsync(new Uri("fabric:/app/service"), updateDescription);
```

<span data-ttu-id="6a2f6-415">Prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="6a2f6-415">Powershell:</span></span>

```posh
Update-ServiceFabricService -Stateful -ServiceName $serviceName -PlacementConstraints "NodeType == NodeType01"
```

<span data-ttu-id="6a2f6-416">Pro všechny různé služby s názvem instance jsou zadány omezení umístění.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-416">Placement constraints are specified for every different named service instance.</span></span> <span data-ttu-id="6a2f6-417">Aktualizace vždy provést místa (Přepsat) byl dříve zadán.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-417">Updates always take the place of (overwrite) what was previously specified.</span></span>

<span data-ttu-id="6a2f6-418">Definice clusteru definuje vlastnosti na uzlu.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-418">The cluster definition defines the properties on a node.</span></span> <span data-ttu-id="6a2f6-419">Změna vlastnosti uzlu vyžaduje s upgradem konfigurace clusteru.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-419">Changing a node's properties requires a cluster configuration upgrade.</span></span> <span data-ttu-id="6a2f6-420">Upgrade vlastnosti uzlu vyžaduje každý ovlivněných uzel restartuje, aby se jeho nové vlastnosti sestav.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-420">Upgrading a node's properties requires each affected node to restart to report its new properties.</span></span> <span data-ttu-id="6a2f6-421">Tyto vrácení upgradů Service Fabric spravuje.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-421">These rolling upgrades are managed by Service Fabric.</span></span>

## <a name="describing-and-managing-cluster-resources"></a><span data-ttu-id="6a2f6-422">Popisující a Správa prostředků clusteru</span><span class="sxs-lookup"><span data-stu-id="6a2f6-422">Describing and Managing Cluster Resources</span></span>
<span data-ttu-id="6a2f6-423">Jedním z nejdůležitějších úlohy žádné orchestrator je ke správě spotřeby prostředků v clusteru.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-423">One of the most important jobs of any orchestrator is to help manage resource consumption in the cluster.</span></span> <span data-ttu-id="6a2f6-424">Správa prostředků clusteru může zahrnovat několik různých věcí.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-424">Managing cluster resources can mean a couple of different things.</span></span> <span data-ttu-id="6a2f6-425">Nejprve existuje zajišťuje, že počítače nejsou přetížené.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-425">First, there's ensuring that machines are not overloaded.</span></span> <span data-ttu-id="6a2f6-426">To znamená, a ujistěte se, že počítače nejsou spuštěné další služby, než se může zpracovat.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-426">This means making sure that machines aren't running more services than they can handle.</span></span> <span data-ttu-id="6a2f6-427">Druhou je, že vyrovnávání a optimalizace, které jsou kritická pro efektivní spuštění služby.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-427">Second, there's balancing and optimization which is critical to running services efficiently.</span></span> <span data-ttu-id="6a2f6-428">Některé uzly je aktivní, zatímco jiné jsou cold nemůže povolit náklady nabídky služeb citlivé platné nebo výkonu.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-428">Cost effective or performance sensitive service offerings can't allow some nodes to be hot while others are cold.</span></span> <span data-ttu-id="6a2f6-429">Aktivní uzly vést k sporu prostředků a nízký výkon a cold uzly představují nevyužité zdroje a vyšší náklady.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-429">Hot nodes lead to resource contention and poor performance, and cold nodes represent wasted resources and increased costs.</span></span> 

<span data-ttu-id="6a2f6-430">Service Fabric představuje prostředky jako `Metrics`.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-430">Service Fabric represents resources as `Metrics`.</span></span> <span data-ttu-id="6a2f6-431">Metriky jsou logickou nebo fyzickou prostředku, který chcete popisují do Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-431">Metrics are any logical or physical resource that you want to describe to Service Fabric.</span></span> <span data-ttu-id="6a2f6-432">Příklady metrik věci jako "WorkQueueDepth" nebo "MemoryInMb".</span><span class="sxs-lookup"><span data-stu-id="6a2f6-432">Examples of metrics are things like “WorkQueueDepth” or “MemoryInMb”.</span></span> <span data-ttu-id="6a2f6-433">Informace o fyzické prostředky, které můžou řídit Service Fabric na uzlech naleznete v tématu [zásad správného řízení prostředků](service-fabric-resource-governance.md).</span><span class="sxs-lookup"><span data-stu-id="6a2f6-433">For information about the physical resources that Service Fabric can govern on nodes, see [resource governance](service-fabric-resource-governance.md).</span></span> <span data-ttu-id="6a2f6-434">Informace o konfiguraci vlastní metriky a jejich použití naleznete v tématu [v tomto článku](service-fabric-cluster-resource-manager-metrics.md)</span><span class="sxs-lookup"><span data-stu-id="6a2f6-434">For information on configuring custom metrics and their uses, see [this article](service-fabric-cluster-resource-manager-metrics.md)</span></span>

<span data-ttu-id="6a2f6-435">Metriky se liší od umísťováním omezení a vlastnosti uzlu.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-435">Metrics are different from placements constraints and node properties.</span></span> <span data-ttu-id="6a2f6-436">Vlastnosti uzlu jsou statické popisovače v samotných uzlech.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-436">Node properties are static descriptors of the nodes themselves.</span></span> <span data-ttu-id="6a2f6-437">Metriky popisují prostředky, které mají uzly a že využívat služby při spuštění na uzlu.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-437">Metrics describe resources that nodes have and that services consume when they are run on a node.</span></span> <span data-ttu-id="6a2f6-438">Vlastnost uzlu může být "HasSSD" a může být nastavena na hodnotu true nebo false.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-438">A node property could be "HasSSD" and could be set to true or false.</span></span> <span data-ttu-id="6a2f6-439">Množství místa, které jsou k dispozici v této SSD a kolik je využívána služby by metrika jako "DriveSpaceInMb".</span><span class="sxs-lookup"><span data-stu-id="6a2f6-439">The amount of space available on that SSD and how much is consumed by services would be a metric like “DriveSpaceInMb”.</span></span> 

<span data-ttu-id="6a2f6-440">Je důležité si uvědomit, že stejně jako omezení umístění a vlastnosti uzlu, správce prostředků clusteru Service Fabric nerozumí co znamená názvy metriky.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-440">It is important to note that just like for placement constraints and node properties, the Service Fabric Cluster Resource Manager doesn't understand what the names of the metrics mean.</span></span> <span data-ttu-id="6a2f6-441">Metriky názvy jsou jenom řetězce.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-441">Metric names are just strings.</span></span> <span data-ttu-id="6a2f6-442">Je dobrým zvykem deklarovat jednotky jako součást metriky názvy, které vytvoříte, když může být nejednoznačný.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-442">It is a good practice to declare units as a part of the metric names that you create when it could be ambiguous.</span></span>

## <a name="capacity"></a><span data-ttu-id="6a2f6-443">Kapacita</span><span class="sxs-lookup"><span data-stu-id="6a2f6-443">Capacity</span></span>
<span data-ttu-id="6a2f6-444">Pokud jste měli vypnuté všechny prostředků *vyrovnávání*, správce prostředků clusteru Service Fabric stále zajistí, že žádný uzel skončila přes své kapacity.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-444">If you turned off all resource *balancing*, Service Fabric’s Cluster Resource Manager would still ensure that no node ended up over its capacity.</span></span> <span data-ttu-id="6a2f6-445">Správa přetečení kapacity je možné, pokud je příliš plná clusteru nebo zatížení je větší než libovolného uzlu.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-445">Managing capacity overruns is possible unless the cluster is too full or the workload is larger than any node.</span></span> <span data-ttu-id="6a2f6-446">Kapacita je jiné *omezení* že zjistit, kolik prostředků a uzel se používá správce prostředků clusteru.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-446">Capacity is another *constraint* that the Cluster Resource Manager uses to understand how much of a resource a node has.</span></span> <span data-ttu-id="6a2f6-447">Zbývající kapacity jsou také sledovány clusteru jako celek.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-447">Remaining capacity is also tracked for the cluster as a whole.</span></span> <span data-ttu-id="6a2f6-448">Kapacita a spotřeba na úrovni služby jsou vyjádřeny z hlediska metriky.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-448">Both the capacity and the consumption at the service level are expressed in terms of metrics.</span></span> <span data-ttu-id="6a2f6-449">Tak například Metrika může být "ClientConnections" a daný uzel je pravděpodobně kapacity pro "ClientConnections" z 32768.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-449">So for example, the metric might be "ClientConnections" and a given Node may have a capacity for "ClientConnections" of 32768.</span></span> <span data-ttu-id="6a2f6-450">Další uzly může mít jiné omezení některé služby spuštěné v tomto uzlu můžete například ho aktuálně spotřebovává 32256 metrika "ClientConnections".</span><span class="sxs-lookup"><span data-stu-id="6a2f6-450">Other nodes can have other limits Some service running on that node can say it is currently consuming 32256 of the metric "ClientConnections".</span></span>

<span data-ttu-id="6a2f6-451">Během doby běhu správce prostředků clusteru sleduje zbývající kapacity v clusteru a na uzlech.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-451">During runtime, the Cluster Resource Manager tracks remaining capacity in the cluster and on nodes.</span></span> <span data-ttu-id="6a2f6-452">Aby bylo možné sledovat kapacity odečítá správce prostředků clusteru využití každé služby z uzlu kapacity, kde je služba spuštěna.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-452">In order to track capacity the Cluster Resource Manager subtracts each service's usage from node's capacity where the service runs.</span></span> <span data-ttu-id="6a2f6-453">Pomocí těchto informací může správce prostředků clusteru Service Fabric rozmyslete si, kam umístit nebo přesunout repliky tak, aby uzly si projít kapacity.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-453">With this information, the Service Fabric Cluster Resource Manager can figure out where to place or move replicas so that nodes don’t go over capacity.</span></span>

<span data-ttu-id="6a2f6-454"><center>
![Uzly clusteru a kapacity][Image7]
</center></span><span class="sxs-lookup"><span data-stu-id="6a2f6-454"><center>
![Cluster nodes and capacity][Image7]
</center></span></span>

<span data-ttu-id="6a2f6-455">C#:</span><span class="sxs-lookup"><span data-stu-id="6a2f6-455">C#:</span></span>

```csharp
StatefulServiceDescription serviceDescription = new StatefulServiceDescription();
ServiceLoadMetricDescription metric = new ServiceLoadMetricDescription();
metric.Name = "ClientConnections";
metric.PrimaryDefaultLoad = 1024;
metric.SecondaryDefaultLoad = 0;
metric.Weight = ServiceLoadMetricWeight.High;
serviceDescription.Metrics.Add(metric);
await fabricClient.ServiceManager.CreateServiceAsync(serviceDescription);
```

<span data-ttu-id="6a2f6-456">Prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="6a2f6-456">Powershell:</span></span>

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton –Metric @("ClientConnections,High,1024,0)
```

<span data-ttu-id="6a2f6-457">Můžete zjistit kapacity definované v manifestu clusteru:</span><span class="sxs-lookup"><span data-stu-id="6a2f6-457">You can see capacities defined in the cluster manifest:</span></span>

<span data-ttu-id="6a2f6-458">ClusterManifest.xml</span><span class="sxs-lookup"><span data-stu-id="6a2f6-458">ClusterManifest.xml</span></span>

```xml
    <NodeType Name="NodeType03">
      <Capacities>
        <Capacity Name="ClientConnections" Value="65536"/>
      </Capacities>
    </NodeType>
```

<span data-ttu-id="6a2f6-459">pomocí souboru ClusterConfig.json pro samostatné nasazení nebo Template.json pro Azure hostované clustery.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-459">via ClusterConfig.json for Standalone deployments or Template.json for Azure hosted clusters.</span></span> 

```json
"nodeTypes": [
    {
        "name": "NodeType03",
        "capacities": {
            "ClientConnections": "65536",
        }
    }
],
```

<span data-ttu-id="6a2f6-460">Běžně služby načíst dynamicky změny.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-460">Commonly a service’s load changes dynamically.</span></span> <span data-ttu-id="6a2f6-461">Řekněme, že zatížení repliky "ClientConnections" změnil z hodnoty 1 024 na 2048, ale na uzel, který byl spuštěn na pak pouze byl 512 kapacita zbývající pro tuto metriku.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-461">Say that a replica's load of "ClientConnections" changed from 1024 to 2048, but the node it was running on then only had 512 capacity remaining for that metric.</span></span> <span data-ttu-id="6a2f6-462">Nyní repliky nebo instance umístění je neplatná, protože v tomto uzlu není dostatek místa.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-462">Now that replica or instance's placement is invalid, since there's not enough room on that node.</span></span> <span data-ttu-id="6a2f6-463">Správce prostředků clusteru má nové a získat uzel zpět níže kapacity.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-463">The Cluster Resource Manager has to kick in and get the node back below capacity.</span></span> <span data-ttu-id="6a2f6-464">Snižuje zatížení na uzlu, který je nad kapacity přesunutím jeden nebo víc replik nebo instancí z tohoto uzlu do dalších uzlů.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-464">It reduces load on the node that is over capacity by moving one or more of the replicas or instances from that node to other nodes.</span></span> <span data-ttu-id="6a2f6-465">Při přesunu repliky, pokusí se správce prostředků clusteru minimalizovat náklady na těchto pohybů.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-465">When moving replicas, the Cluster Resource Manager tries to minimize the cost of those movements.</span></span> <span data-ttu-id="6a2f6-466">Náklady na přesunutí popsané v [v tomto článku](service-fabric-cluster-resource-manager-movement-cost.md) a informace o správce prostředků clusteru je vyrovnává strategie a pravidla je popsaný [zde](service-fabric-cluster-resource-manager-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="6a2f6-466">Movement cost is discussed in [this article](service-fabric-cluster-resource-manager-movement-cost.md) and more about the Cluster Resource Manager's rebalancing strategies and rules is described [here](service-fabric-cluster-resource-manager-metrics.md).</span></span>

## <a name="cluster-capacity"></a><span data-ttu-id="6a2f6-467">Kapacita clusteru</span><span class="sxs-lookup"><span data-stu-id="6a2f6-467">Cluster capacity</span></span>
<span data-ttu-id="6a2f6-468">Jak tedy správce prostředků clusteru Service Fabric zachovat celkové clusteru z příliš zaplnění?</span><span class="sxs-lookup"><span data-stu-id="6a2f6-468">So how does the Service Fabric Cluster Resource Manager keep the overall cluster from being too full?</span></span> <span data-ttu-id="6a2f6-469">Dobře s dynamického zatížení, není k dispozici mnoho ho provést.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-469">Well, with dynamic load there’s not a lot it can do.</span></span> <span data-ttu-id="6a2f6-470">Služby mohou mít jejich zatížení Špička nezávisle na akce prováděné pomocí Správce prostředků clusteru.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-470">Services can have their load spike independently of actions taken by the Cluster Resource Manager.</span></span> <span data-ttu-id="6a2f6-471">Cluster s dostatek prostoru dnes v důsledku toho může být underpowered, když jste se famous zítra.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-471">As a result, your cluster with plenty of headroom today may be underpowered when you become famous tomorrow.</span></span> <span data-ttu-id="6a2f6-472">Ale nutné dodat, nejsou některé ovládací prvky, které jsou v zaručená, aby se zabránilo problémům.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-472">That said, there are some controls that are baked in to prevent problems.</span></span> <span data-ttu-id="6a2f6-473">První věcí, kterou by bylo možné službu je bránit vytvoření nové úlohy, které by způsobily clusteru plná.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-473">The first thing we can do is prevent the creation of new workloads that would cause the cluster to become full.</span></span>

<span data-ttu-id="6a2f6-474">Řekněme, že vytvoření bezstavové služby a obsahuje některé zatížení s ním spojená.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-474">Say that you create a stateless service and it has some load associated with it.</span></span> <span data-ttu-id="6a2f6-475">Řekněme, že služba záleží metrika "DiskSpaceInMb".</span><span class="sxs-lookup"><span data-stu-id="6a2f6-475">Let’s say that the service cares about the "DiskSpaceInMb" metric.</span></span> <span data-ttu-id="6a2f6-476">Dále předpokládejme, že se bude používat pět jednotky "DiskSpaceInMb" pro všechny instance služby.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-476">Let's also say that it is going to consume five units of "DiskSpaceInMb" for every instance of the service.</span></span> <span data-ttu-id="6a2f6-477">Chcete vytvořit tři instancí služby.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-477">You want to create three instances of the service.</span></span> <span data-ttu-id="6a2f6-478">Výborně!</span><span class="sxs-lookup"><span data-stu-id="6a2f6-478">Great!</span></span> <span data-ttu-id="6a2f6-479">To znamená, že potřebujeme 15 jednotky "DiskSpaceInMb" nacházet v clusteru, abychom vás mohli i tak nebude moci vytvořit tyto instance služby.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-479">So that means that we need 15 units of "DiskSpaceInMb" to be present in the cluster in order for us to even be able to create these service instances.</span></span> <span data-ttu-id="6a2f6-480">Správce prostředků clusteru průběžně vypočítá kapacitu a využití každé metriky tak může zjistit, zbývající kapacity v clusteru.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-480">The Cluster Resource Manager continually calculates the capacity and consumption of each metric so it can determine the remaining capacity in the cluster.</span></span> <span data-ttu-id="6a2f6-481">Pokud není k dispozici dostatek místa, správce prostředků clusteru odmítne volání služby vytvořit.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-481">If there isn't enough space, the Cluster Resource Manager rejects the create service call.</span></span>

<span data-ttu-id="6a2f6-482">Vzhledem k tomu, že požadavek je pouze to, že existuje možné 15 jednotky, může být tento prostor přiděleno mnoha různými způsoby.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-482">Since the requirement is only that there be 15 units available, this space could be allocated many different ways.</span></span> <span data-ttu-id="6a2f6-483">Například může existovat jednu jednotku zbývající kapacity v 15 rozdílných uzlech, nebo tři zbývající jednotky kapacity na pět různých uzlech.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-483">For example, there could be one remaining unit of capacity on 15 different nodes, or three remaining units of capacity on five different nodes.</span></span> <span data-ttu-id="6a2f6-484">Pokud správce prostředků clusteru můžete přeuspořádat věcí, takže na tři uzly je k dispozici pět jednotky, umístí službu.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-484">If the Cluster Resource Manager can rearrange things so there's five units available on three nodes, it places the service.</span></span> <span data-ttu-id="6a2f6-485">Změna uspořádání clusteru je obvykle možné, pokud je téměř plná clusteru, nebo z nějakého důvodu nelze konsolidovat stávající služby.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-485">Rearranging the cluster is usually possible unless the cluster is almost full or the existing services can't be consolidated for some reason.</span></span>

## <a name="buffered-capacity"></a><span data-ttu-id="6a2f6-486">Kapacita ve vyrovnávací paměti</span><span class="sxs-lookup"><span data-stu-id="6a2f6-486">Buffered Capacity</span></span>
<span data-ttu-id="6a2f6-487">Kapacita ve vyrovnávací paměti je jiné funkce služby Správce prostředků clusteru.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-487">Buffered capacity is another feature of the Cluster Resource Manager.</span></span> <span data-ttu-id="6a2f6-488">To umožňuje rezervace některé část celkové kapacity uzlu.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-488">It allows reservation of some portion of the overall node capacity.</span></span> <span data-ttu-id="6a2f6-489">Této kapacity vyrovnávací paměti se používá pouze k umístění služby během upgradu a selhání uzlu.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-489">This capacity buffer is only used to place services during upgrades and node failures.</span></span> <span data-ttu-id="6a2f6-490">Kapacita ve vyrovnávací paměti je zadán globálně za Metrika pro všechny uzly.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-490">Buffered Capacity is specified globally per metric for all nodes.</span></span> <span data-ttu-id="6a2f6-491">Hodnota, kterou vyberete pro rezervované kapacity je funkce, počtu selhání a upgradu domén, které máte v clusteru.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-491">The value you pick for the reserved capacity is a function of the number of Fault and Upgrade Domains you have in the cluster.</span></span> <span data-ttu-id="6a2f6-492">Další selhání a upgradu domény znamená, můžete si vybrat nižší číslo pro vaše kapacita ve vyrovnávací paměti.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-492">More Fault and Upgrade Domains means that you can pick a lower number for your buffered capacity.</span></span> <span data-ttu-id="6a2f6-493">Pokud máte více domén, můžete očekávat snížení objemu clusteru a mohli být k dispozici během upgradu a selhání.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-493">If you have more domains, you can expect smaller amounts of your cluster to be unavailable during upgrades and failures.</span></span> <span data-ttu-id="6a2f6-494">Zadání uložená do vyrovnávací paměti kapacity má smysl pouze, pokud zadáte uzlu kapacity pro metriku.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-494">Specifying Buffered Capacity only makes sense if you have also specified the node capacity for a metric.</span></span>

<span data-ttu-id="6a2f6-495">Tady je příklad toho, jak určit kapacitu ve vyrovnávací paměti:</span><span class="sxs-lookup"><span data-stu-id="6a2f6-495">Here's an example of how to specify buffered capacity:</span></span>

<span data-ttu-id="6a2f6-496">ClusterManifest.xml</span><span class="sxs-lookup"><span data-stu-id="6a2f6-496">ClusterManifest.xml</span></span>

```xml
        <Section Name="NodeBufferPercentage">
            <Parameter Name="SomeMetric" Value="0.15" />
            <Parameter Name="SomeOtherMetric" Value="0.20" />
        </Section>
```

<span data-ttu-id="6a2f6-497">pomocí souboru ClusterConfig.json pro samostatné nasazení nebo Template.json pro Azure hostované clustery:</span><span class="sxs-lookup"><span data-stu-id="6a2f6-497">via ClusterConfig.json for Standalone deployments or Template.json for Azure hosted clusters:</span></span>

```json
"fabricSettings": [
  {
    "name": "NodeBufferPercentage",
    "parameters": [
      {
          "name": "SomeMetric",
          "value": "0.15"
      },
      {
          "name": "SomeOtherMetric",
          "value": "0.20"
      }
    ]
  }
]
```

<span data-ttu-id="6a2f6-498">Vytvoření nové služby selže, když clusteru je mimo ve vyrovnávací paměti kapacity pro metriku.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-498">The creation of new services fails when the cluster is out of buffered capacity for a metric.</span></span> <span data-ttu-id="6a2f6-499">Brání vytvoření nových služeb, aby byla zachována vyrovnávací paměti zajišťuje upgrady a selhání nezpůsobí uzly si projít kapacity.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-499">Preventing the creation of new services to preserve the buffer ensures that upgrades and failures don’t cause nodes to go over capacity.</span></span> <span data-ttu-id="6a2f6-500">Kapacita ve vyrovnávací paměti je nepovinný, ale doporučuje se v jakékoli clusteru, který definuje kapacity pro metriku.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-500">Buffered capacity is optional but is recommended in any cluster that defines a capacity for a metric.</span></span>

<span data-ttu-id="6a2f6-501">Správce prostředků clusteru zpřístupní tyto informace zatížení.</span><span class="sxs-lookup"><span data-stu-id="6a2f6-501">The Cluster Resource Manager exposes this load information.</span></span> <span data-ttu-id="6a2f6-502">Pro jednotlivé metriky tyto informace zahrnují:</span><span class="sxs-lookup"><span data-stu-id="6a2f6-502">For each metric, this information includes:</span></span> 
  - <span data-ttu-id="6a2f6-503">Nastavení kapacity ve vyrovnávací paměti</span><span class="sxs-lookup"><span data-stu-id="6a2f6-503">the buffered capacity settings</span></span>
  - <span data-ttu-id="6a2f6-504">Celková kapacita</span><span class="sxs-lookup"><span data-stu-id="6a2f6-504">the total capacity</span></span>
  - <span data-ttu-id="6a2f6-505">aktuální spotřeba</span><span class="sxs-lookup"><span data-stu-id="6a2f6-505">the current consumption</span></span>
  - <span data-ttu-id="6a2f6-506">jestli je považován za jednotlivé metriky vyrovnáváním nebo ne</span><span class="sxs-lookup"><span data-stu-id="6a2f6-506">whether each metric is considered balanced or not</span></span>
  - <span data-ttu-id="6a2f6-507">Statistika týkající se směrodatné odchylky</span><span class="sxs-lookup"><span data-stu-id="6a2f6-507">statistics about the standard deviation</span></span>
  - <span data-ttu-id="6a2f6-508">uzly, které mají nejvíce a minimálně zatížení</span><span class="sxs-lookup"><span data-stu-id="6a2f6-508">the nodes which have the most and least load</span></span>  
  
<span data-ttu-id="6a2f6-509">Níže vidíte příklad tento výstup:</span><span class="sxs-lookup"><span data-stu-id="6a2f6-509">Below we see an example of that output:</span></span>

```posh
PS C:\Users\user> Get-ServiceFabricClusterLoadInformation
LastBalancingStartTimeUtc : 9/1/2016 12:54:59 AM
LastBalancingEndTimeUtc   : 9/1/2016 12:54:59 AM
LoadMetricInformation     :
                            LoadMetricName        : Metric1
                            IsBalancedBefore      : False
                            IsBalancedAfter       : False
                            DeviationBefore       : 0.192450089729875
                            DeviationAfter        : 0.192450089729875
                            BalancingThreshold    : 1
                            Action                : NoActionNeeded
                            ActivityThreshold     : 0
                            ClusterCapacity       : 189
                            ClusterLoad           : 45
                            ClusterRemainingCapacity : 144
                            NodeBufferPercentage  : 10
                            ClusterBufferedCapacity : 170
                            ClusterRemainingBufferedCapacity : 125
                            ClusterCapacityViolation : False
                            MinNodeLoadValue      : 0
                            MinNodeLoadNodeId     : 3ea71e8e01f4b0999b121abcbf27d74d
                            MaxNodeLoadValue      : 15
                            MaxNodeLoadNodeId     : 2cc648b6770be1bc9824fa995d5b68b1
```

## <a name="next-steps"></a><span data-ttu-id="6a2f6-510">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6a2f6-510">Next steps</span></span>
* <span data-ttu-id="6a2f6-511">Informace o architektuře a informačního toku v rámci správce prostředků clusteru, podívejte se na [v tomto článku](service-fabric-cluster-resource-manager-architecture.md)</span><span class="sxs-lookup"><span data-stu-id="6a2f6-511">For information on the architecture and information flow within the Cluster Resource Manager, check out [this article ](service-fabric-cluster-resource-manager-architecture.md)</span></span>
* <span data-ttu-id="6a2f6-512">Definování defragmentace metriky je jeden způsob, jak konsolidovat zatížení na uzlech místo rozšíří ho. Naučte se konfigurovat defragmentace, najdete v tématu [v tomto článku](service-fabric-cluster-resource-manager-defragmentation-metrics.md)</span><span class="sxs-lookup"><span data-stu-id="6a2f6-512">Defining Defragmentation Metrics is one way to consolidate load on nodes instead of spreading it out. To learn how to configure defragmentation, refer to [this article](service-fabric-cluster-resource-manager-defragmentation-metrics.md)</span></span>
* <span data-ttu-id="6a2f6-513">Začít od začátku a [získejte Úvod do Service Fabric clusteru správce prostředků](service-fabric-cluster-resource-manager-introduction.md)</span><span class="sxs-lookup"><span data-stu-id="6a2f6-513">Start from the beginning and [get an Introduction to the Service Fabric Cluster Resource Manager](service-fabric-cluster-resource-manager-introduction.md)</span></span>
* <span data-ttu-id="6a2f6-514">Chcete-li zjistit, o tom, jak správce prostředků clusteru spravuje a vyrovnává zatížení v clusteru, podívejte se na článek na [Vyrovnávání zatížení](service-fabric-cluster-resource-manager-balancing.md)</span><span class="sxs-lookup"><span data-stu-id="6a2f6-514">To find out about how the Cluster Resource Manager manages and balances load in the cluster, check out the article on [balancing load](service-fabric-cluster-resource-manager-balancing.md)</span></span>

[Image1]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-fault-domains.png
[Image2]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-uneven-fault-domain-layout.png
[Image3]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-fault-and-upgrade-domains-with-placement.png
[Image4]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-fault-and-upgrade-domain-layout-strategies.png
[Image5]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-layout-different-workloads.png
[Image6]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-placement-constraints-node-properties.png
[Image7]:./media/service-fabric-cluster-resource-manager-cluster-description/cluster-nodes-and-capacity.png
