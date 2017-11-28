---
title: "aaaAzure instancí kontejnerů a kontejner Orchestration"
description: "Pochopit, jak Azure kontejner instancí interakci s orchestrators kontejneru"
services: container-instances
documentationcenter: 
author: seanmck
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 
ms.service: container-instances
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/24/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 69a39edc6f14d885c1ac300990ed1399002ccfee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-container-instances-and-container-orchestrators"></a><span data-ttu-id="f8032-103">Azure instancí kontejnerů a kontejner orchestrators</span><span class="sxs-lookup"><span data-stu-id="f8032-103">Azure Container Instances and container orchestrators</span></span>

<span data-ttu-id="f8032-104">Kvůli jejich malá velikost a orientaci aplikace je výhodné pro agilní doručení prostředí a na základě mikroslužbu architektury kontejnery.</span><span class="sxs-lookup"><span data-stu-id="f8032-104">Because of their small size and application orientation, containers are well suited for agile delivery environments and microservice-based architectures.</span></span> <span data-ttu-id="f8032-105">Hello úlohy automatizace a správa velký počet kontejnerů a jejich vzájemné interakce se označuje jako *orchestration*.</span><span class="sxs-lookup"><span data-stu-id="f8032-105">hello task of automating and managing a large number of containers and how they interact is known as *orchestration*.</span></span> <span data-ttu-id="f8032-106">Oblíbených kontejneru orchestrators zahrnují Kubernetes, DC/OS a Docker Swarm, které jsou k dispozici v hello [Azure Container Service](https://docs.microsoft.com/azure/container-service/).</span><span class="sxs-lookup"><span data-stu-id="f8032-106">Popular container orchestrators include Kubernetes, DC/OS, and Docker Swarm, all of which are available in hello [Azure Container Service](https://docs.microsoft.com/azure/container-service/).</span></span>

<span data-ttu-id="f8032-107">Azure instancí kontejneru obsahuje některé z hello základní plánovací orchestration platforem, ale nezahrnuje hello vyšší hodnota služby, aby tyto platformy poskytují a může být ve skutečnosti doplňkové s nimi.</span><span class="sxs-lookup"><span data-stu-id="f8032-107">Azure Container Instances provides some of hello basic scheduling capabilities of orchestration platforms, but it does not cover hello higher-value services that those platforms provide and can in fact be complementary with them.</span></span> <span data-ttu-id="f8032-108">Tento článek popisuje hello oboru co zpracovává instancí kontejnerů Azure a jak úplné orchestrators kontejner může pracovat s ním.</span><span class="sxs-lookup"><span data-stu-id="f8032-108">This article describes hello scope of what Azure Container Instances handles and how full container orchestrators might interact with it.</span></span>

## <a name="traditional-orchestration"></a><span data-ttu-id="f8032-109">Tradiční orchestration</span><span class="sxs-lookup"><span data-stu-id="f8032-109">Traditional orchestration</span></span>

<span data-ttu-id="f8032-110">standardní definice Hello Orchestrace zahrnuje hello následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="f8032-110">hello standard definition of orchestration includes hello following tasks:</span></span>

- <span data-ttu-id="f8032-111">**Plánování**: daného kontejneru image a žádost o prostředek, najít vhodný počítač, na který toorun hello kontejner.</span><span class="sxs-lookup"><span data-stu-id="f8032-111">**Scheduling**: Given a container image and a resource request, find a suitable machine on which toorun hello container.</span></span>
- <span data-ttu-id="f8032-112">**Spřažení/proti-affinity**: určení, že by měl sadu kontejnery spuštěny nedaleko druhou (pro výkon) nebo dostatečně daleko od sebe (pro dostupnost).</span><span class="sxs-lookup"><span data-stu-id="f8032-112">**Affinity/Anti-affinity**: Specify that a set of containers should run nearby each other (for performance) or sufficiently far apart (for availability).</span></span>
- <span data-ttu-id="f8032-113">**Sledování stavu**: Podívejte se na chyby kontejneru a automaticky znovu je naplánujte.</span><span class="sxs-lookup"><span data-stu-id="f8032-113">**Health monitoring**: Watch for container failures and automatically reschedule them.</span></span>
- <span data-ttu-id="f8032-114">**Převzetí služeb při selhání**: sledovat co běží na každém počítači a změnit plán naplánovaných kontejnery z uzlů toohealthy počítače se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="f8032-114">**Failover**: Keep track of what is running on each machine and reschedule containers from failed machines toohealthy nodes.</span></span>
- <span data-ttu-id="f8032-115">**Škálování**: Přidání nebo odebrání kontejneru instancí toomatch vyžádání, ručně nebo automaticky.</span><span class="sxs-lookup"><span data-stu-id="f8032-115">**Scaling**: Add or remove container instances toomatch demand, either manually or automatically.</span></span>
- <span data-ttu-id="f8032-116">**Sítě**: zadejte překryvné sítě pro spolupráci kontejnery toocommunicate napříč více hostitelských počítačích.</span><span class="sxs-lookup"><span data-stu-id="f8032-116">**Networking**: Provide an overlay network for coordinating containers toocommunicate across multiple host machines.</span></span>
- <span data-ttu-id="f8032-117">**Zjišťování služby**: Povolit kontejnery toolocate navzájem automaticky to i v případě jejich přechodu mezi hostitele počítače a změnit IP adresy.</span><span class="sxs-lookup"><span data-stu-id="f8032-117">**Service discovery**: Enable containers toolocate each other automatically even as they move between host machines and change IP addresses.</span></span>
- <span data-ttu-id="f8032-118">**Koordinované upgradů aplikací**: Spravovat aplikace tooavoid upgrady kontejnerů výpadek a povolte vrácení zpět, pokud dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="f8032-118">**Coordinated application upgrades**: Manage container upgrades tooavoid application down time and enable rollback if something goes wrong.</span></span>

## <a name="orchestration-with-azure-container-instances-a-layered-approach"></a><span data-ttu-id="f8032-119">Orchestration s instancemi Azure kontejneru: vrstveného přístupu</span><span class="sxs-lookup"><span data-stu-id="f8032-119">Orchestration with Azure Container Instances: A layered approach</span></span>

<span data-ttu-id="f8032-120">Azure instancí kontejnerů umožňuje tooorchestration vrstveného přístupu poskytuje všechny hello plánování a možnosti správy vyžaduje toorun jediný kontejner, zatímco orchestrator platformy toomanage více kontejneru úlohy nad jeho.</span><span class="sxs-lookup"><span data-stu-id="f8032-120">Azure Container Instances enables a layered approach tooorchestration, providing all of hello scheduling and management capabilities required toorun a single container, while allowing orchestrator platforms toomanage multi-container tasks on top of it.</span></span>

<span data-ttu-id="f8032-121">Protože všechny hello základní infrastruktury pro kontejner instancí je spravovaná službou Azure, nemusí platformě orchestrator s hledání příslušný hostitelský počítač, na které toorun jediný kontejner tooconcern sám sebe.</span><span class="sxs-lookup"><span data-stu-id="f8032-121">Because all of hello underlying infrastructure for Container Instances is managed by Azure, an orchestrator platform does not need tooconcern itself with finding an appropriate host machine on which toorun a single container.</span></span> <span data-ttu-id="f8032-122">pružnost Hello hello cloudu zajistí, že než je vždy k dispozici.</span><span class="sxs-lookup"><span data-stu-id="f8032-122">hello elasticity of hello cloud ensures that one is always available.</span></span> <span data-ttu-id="f8032-123">Místo toho hello orchestrator můžete soustředit na hello úlohy, které zjednodušují vývoj hello architektury více kontejneru, včetně změny a koordinované upgrady.</span><span class="sxs-lookup"><span data-stu-id="f8032-123">Instead, hello orchestrator can focus on hello tasks that simplify hello development of multi-container architectures, including scaling and coordinated upgrades.</span></span>



## <a name="potential-scenarios"></a><span data-ttu-id="f8032-124">Možných scénářů</span><span class="sxs-lookup"><span data-stu-id="f8032-124">Potential scenarios</span></span>

<span data-ttu-id="f8032-125">Sice stále nascent orchestrator integrace s instancemi Azure kontejneru, Očekáváme, že můžou vznikat v několika různých prostředích:</span><span class="sxs-lookup"><span data-stu-id="f8032-125">While orchestrator integration with Azure Container Instances is still nascent, we anticipate that a few different environments may emerge:</span></span>

### <a name="orchestration-of-container-instances-exclusively"></a><span data-ttu-id="f8032-126">Orchestration instancí kontejneru výhradně</span><span class="sxs-lookup"><span data-stu-id="f8032-126">Orchestration of Container Instances exclusively</span></span>

<span data-ttu-id="f8032-127">Protože rychle začít a účtovat podle hello druhé, prostředí založené výhradně na kontejner instancí Azure nabízí hello nejrychlejší způsob, jak tooget spuštění a toodeal s vysoce proměnné úlohy.</span><span class="sxs-lookup"><span data-stu-id="f8032-127">Because they start quickly and bill by hello second, an environment based exclusively on Azure Container Instances offers hello fastest way tooget started and toodeal with highly variable workloads.</span></span>

### <a name="combination-of-container-instances-and-containers-in-virtual-machines"></a><span data-ttu-id="f8032-128">Kombinace instancí kontejnerů a kontejnery v virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="f8032-128">Combination of Container Instances and Containers in Virtual Machines</span></span>

<span data-ttu-id="f8032-129">Pro dlouhodobé, budou za stabilní úlohy, Orchestrace kontejnery v clusteru s podporou vyhrazených virtuálních počítačů obvykle levnější než systémem hello stejné kontejnery s instancí kontejnerů.</span><span class="sxs-lookup"><span data-stu-id="f8032-129">For long-running, stable workloads, orchestrating containers in a cluster of dedicated virtual machines will typically be cheaper than running hello same containers with Container Instances.</span></span> <span data-ttu-id="f8032-130">Kontejner instancí však nabízí vynikající řešení pro rychlé rozšiřování a smluvní vaší toodeal celkovou kapacitu s neočekávanou nebo krátkodobou špičky využití.</span><span class="sxs-lookup"><span data-stu-id="f8032-130">However, Container Instances offer a great solution for quickly expanding and contracting your overall capacity toodeal with unexpected or short-lived spikes in usage.</span></span> <span data-ttu-id="f8032-131">Místo škálování hello počet virtuálních počítačů v clusteru, pak nasazení další kontejnery na tyto počítače, hello orchestrator můžete jednoduše naplánovat hello další kontejnery pomocí kontejner instancí a odstranit, když jsou žádné nebudete potřebovat.</span><span class="sxs-lookup"><span data-stu-id="f8032-131">Rather than scaling out hello number of virtual machines in your cluster, then deploying additional containers onto those machines, hello orchestrator can simply schedule hello additional containers using Container Instances and delete them when they're no longer needed.</span></span>

## <a name="sample-implementation-azure-container-instances-connector-for-kubernetes"></a><span data-ttu-id="f8032-132">Ukázka implementace: Azure kontejner instancí konektor pro Kubernetes</span><span class="sxs-lookup"><span data-stu-id="f8032-132">Sample implementation: Azure Container Instances Connector for Kubernetes</span></span>

<span data-ttu-id="f8032-133">jsme spustili toodemonstrate jak integrovat platformy orchestration kontejner s instancemi Azure kontejneru, sestavování [ukázka konektor pro Kubernetes][aci-connector-k8s].</span><span class="sxs-lookup"><span data-stu-id="f8032-133">toodemonstrate how container orchestration platforms can integrate with Azure Container Instances, we have started building a [sample connector for Kubernetes][aci-connector-k8s].</span></span> 

<span data-ttu-id="f8032-134">Hello konektor pro Kubernetes napodobuje hello [kubelet] [ kubelet-doc] registrace jako uzel neomezená kapacitu a odeslání hello vytvoření [pracovními stanicemi soustředěnými kolem] [ pod-doc] kontejneru a skupiny v Azure kontejner instancí.</span><span class="sxs-lookup"><span data-stu-id="f8032-134">hello connector for Kubernetes mimics hello [kubelet][kubelet-doc] by registering as a node with unlimited capacity and dispatching hello creation of [pods][pod-doc] as container groups in Azure Container Instances.</span></span> 

<!-- ![ACI Connector for Kubernetes][aci-connector-k8s-gif] -->

<span data-ttu-id="f8032-135">Konektory pro ostatní orchestrators může být postavená, podobně jako integrovaná se službami platformy primitiv toocombine hello napájení nástroje orchestrator hello rozhraní API pomocí hello rychlost a zjednodušení správy kontejnerů v Azure kontejner instancí.</span><span class="sxs-lookup"><span data-stu-id="f8032-135">Connectors for other orchestrators could be built that similarly integrated with platform primitives toocombine hello power of hello orchestrator API with hello speed and simplicity of managing containers in Azure Container Instances.</span></span>

> [!WARNING]
> <span data-ttu-id="f8032-136">Hello ACI konektor pro Kubernetes je *experimentální* a neměl by se používat v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="f8032-136">hello ACI connector for Kubernetes is *experimental* and should not be used in production.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f8032-137">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f8032-137">Next steps</span></span>

<span data-ttu-id="f8032-138">Vytvoření vaší první kontejneru Azure kontejner s instancemi s využitím hello [úvodní příručka](container-instances-quickstart.md).</span><span class="sxs-lookup"><span data-stu-id="f8032-138">Create your first container with Azure Container Instances using hello [quick start guide](container-instances-quickstart.md).</span></span>

<!-- IMAGES -->
[aci-connector-k8s-gif]: ./media/container-instances-orchestrator-relationship/aci-connector-k8s.gif

<!-- LINKS -->
[aci-connector-k8s]: https://github.com/azure/aci-connector-k8s
[kubelet-doc]: https://kubernetes.io/docs/admin/kubelet/
[pod-doc]: https://kubernetes.io/docs/concepts/workloads/pods/pod/