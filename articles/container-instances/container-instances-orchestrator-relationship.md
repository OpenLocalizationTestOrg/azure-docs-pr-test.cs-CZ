---
title: "Instancí Azure kontejnerů a kontejner Orchestration"
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
ms.openlocfilehash: cbb558a92d565759c8dc7d2693960955eb053b0a
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="azure-container-instances-and-container-orchestrators"></a><span data-ttu-id="fbcce-103">Azure instancí kontejnerů a kontejner orchestrators</span><span class="sxs-lookup"><span data-stu-id="fbcce-103">Azure Container Instances and container orchestrators</span></span>

<span data-ttu-id="fbcce-104">Kvůli jejich malá velikost a orientaci aplikace je výhodné pro agilní doručení prostředí a na základě mikroslužbu architektury kontejnery.</span><span class="sxs-lookup"><span data-stu-id="fbcce-104">Because of their small size and application orientation, containers are well suited for agile delivery environments and microservice-based architectures.</span></span> <span data-ttu-id="fbcce-105">Úlohu automatizace a správa velký počet kontejnerů a jejich vzájemné interakce se označuje jako *orchestration*.</span><span class="sxs-lookup"><span data-stu-id="fbcce-105">The task of automating and managing a large number of containers and how they interact is known as *orchestration*.</span></span> <span data-ttu-id="fbcce-106">Oblíbených kontejneru orchestrators zahrnují Kubernetes, DC/OS a Docker Swarm, které jsou k dispozici v [Azure Container Service](https://docs.microsoft.com/azure/container-service/).</span><span class="sxs-lookup"><span data-stu-id="fbcce-106">Popular container orchestrators include Kubernetes, DC/OS, and Docker Swarm, all of which are available in the [Azure Container Service](https://docs.microsoft.com/azure/container-service/).</span></span>

<span data-ttu-id="fbcce-107">Azure instancí kontejneru obsahuje některé základní funkce plánování orchestration platforem, ale nezahrnuje služby vyšší hodnota, že tyto platformy poskytují a může být ve skutečnosti doplňkové s nimi.</span><span class="sxs-lookup"><span data-stu-id="fbcce-107">Azure Container Instances provides some of the basic scheduling capabilities of orchestration platforms, but it does not cover the higher-value services that those platforms provide and can in fact be complementary with them.</span></span> <span data-ttu-id="fbcce-108">Tento článek popisuje oboru co zpracovává instancí kontejnerů Azure a jak úplné orchestrators kontejner může pracovat s ním.</span><span class="sxs-lookup"><span data-stu-id="fbcce-108">This article describes the scope of what Azure Container Instances handles and how full container orchestrators might interact with it.</span></span>

## <a name="traditional-orchestration"></a><span data-ttu-id="fbcce-109">Tradiční orchestration</span><span class="sxs-lookup"><span data-stu-id="fbcce-109">Traditional orchestration</span></span>

<span data-ttu-id="fbcce-110">Standardní definice orchestration zahrnuje následující úkoly:</span><span class="sxs-lookup"><span data-stu-id="fbcce-110">The standard definition of orchestration includes the following tasks:</span></span>

- <span data-ttu-id="fbcce-111">**Plánování**: daného kontejneru image a žádost o prostředek, najít vhodný počítač, na který se má spustit kontejneru.</span><span class="sxs-lookup"><span data-stu-id="fbcce-111">**Scheduling**: Given a container image and a resource request, find a suitable machine on which to run the container.</span></span>
- <span data-ttu-id="fbcce-112">**Spřažení/proti-affinity**: určení, že by měl sadu kontejnery spuštěny nedaleko druhou (pro výkon) nebo dostatečně daleko od sebe (pro dostupnost).</span><span class="sxs-lookup"><span data-stu-id="fbcce-112">**Affinity/Anti-affinity**: Specify that a set of containers should run nearby each other (for performance) or sufficiently far apart (for availability).</span></span>
- <span data-ttu-id="fbcce-113">**Sledování stavu**: Podívejte se na chyby kontejneru a automaticky znovu je naplánujte.</span><span class="sxs-lookup"><span data-stu-id="fbcce-113">**Health monitoring**: Watch for container failures and automatically reschedule them.</span></span>
- <span data-ttu-id="fbcce-114">**Převzetí služeb při selhání**: sledovat co běží na každém počítači a změnit plán naplánovaných kontejnery z počítačů se nezdařilo pro uzly v pořádku.</span><span class="sxs-lookup"><span data-stu-id="fbcce-114">**Failover**: Keep track of what is running on each machine and reschedule containers from failed machines to healthy nodes.</span></span>
- <span data-ttu-id="fbcce-115">**Škálování**: Přidání nebo odebrání kontejneru instancí tak, aby odpovídaly vyžádání, ručně nebo automaticky.</span><span class="sxs-lookup"><span data-stu-id="fbcce-115">**Scaling**: Add or remove container instances to match demand, either manually or automatically.</span></span>
- <span data-ttu-id="fbcce-116">**Sítě**: zadejte překryvné sítě pro spolupráci kontejnery pro komunikaci mezi více počítačů hostitele.</span><span class="sxs-lookup"><span data-stu-id="fbcce-116">**Networking**: Provide an overlay network for coordinating containers to communicate across multiple host machines.</span></span>
- <span data-ttu-id="fbcce-117">**Zjišťování služby**: Povolit kontejnery navzájem to i v případě jejich přechodu mezi hostitele počítače a změnit IP adres automaticky vyhledat.</span><span class="sxs-lookup"><span data-stu-id="fbcce-117">**Service discovery**: Enable containers to locate each other automatically even as they move between host machines and change IP addresses.</span></span>
- <span data-ttu-id="fbcce-118">**Koordinované upgradů aplikací**: Správa kontejneru upgrady vyhnout aplikace výpadek a povolte vrácení zpět, pokud dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="fbcce-118">**Coordinated application upgrades**: Manage container upgrades to avoid application down time and enable rollback if something goes wrong.</span></span>

## <a name="orchestration-with-azure-container-instances-a-layered-approach"></a><span data-ttu-id="fbcce-119">Orchestration s instancemi Azure kontejneru: vrstveného přístupu</span><span class="sxs-lookup"><span data-stu-id="fbcce-119">Orchestration with Azure Container Instances: A layered approach</span></span>

<span data-ttu-id="fbcce-120">Azure instancí kontejnerů umožňuje vrstveného přístupu k orchestration, poskytuje všechny funkce správy a plánování, které jsou nutná k provozování jediný kontejner, současně platformy orchestrator ke správě více kontejneru úloh nad jeho.</span><span class="sxs-lookup"><span data-stu-id="fbcce-120">Azure Container Instances enables a layered approach to orchestration, providing all of the scheduling and management capabilities required to run a single container, while allowing orchestrator platforms to manage multi-container tasks on top of it.</span></span>

<span data-ttu-id="fbcce-121">Protože všechny základní infrastruktury pro kontejner instancí je spravovaná službou Azure, není potřeba platformě orchestrator zabývat se hledání příslušné hostitelský počítač, na který se má spustit jeden kontejner.</span><span class="sxs-lookup"><span data-stu-id="fbcce-121">Because all of the underlying infrastructure for Container Instances is managed by Azure, an orchestrator platform does not need to concern itself with finding an appropriate host machine on which to run a single container.</span></span> <span data-ttu-id="fbcce-122">Pružnost cloudu zajistí, že než je vždy k dispozici.</span><span class="sxs-lookup"><span data-stu-id="fbcce-122">The elasticity of the cloud ensures that one is always available.</span></span> <span data-ttu-id="fbcce-123">Místo toho orchestrator můžete soustředit na úlohy, které zjednodušují vývoj architektury více kontejneru, včetně změny a koordinované upgrady.</span><span class="sxs-lookup"><span data-stu-id="fbcce-123">Instead, the orchestrator can focus on the tasks that simplify the development of multi-container architectures, including scaling and coordinated upgrades.</span></span>



## <a name="potential-scenarios"></a><span data-ttu-id="fbcce-124">Možných scénářů</span><span class="sxs-lookup"><span data-stu-id="fbcce-124">Potential scenarios</span></span>

<span data-ttu-id="fbcce-125">Sice stále nascent orchestrator integrace s instancemi Azure kontejneru, Očekáváme, že můžou vznikat v několika různých prostředích:</span><span class="sxs-lookup"><span data-stu-id="fbcce-125">While orchestrator integration with Azure Container Instances is still nascent, we anticipate that a few different environments may emerge:</span></span>

### <a name="orchestration-of-container-instances-exclusively"></a><span data-ttu-id="fbcce-126">Orchestration instancí kontejneru výhradně</span><span class="sxs-lookup"><span data-stu-id="fbcce-126">Orchestration of Container Instances exclusively</span></span>

<span data-ttu-id="fbcce-127">Protože rychle začít a účtovat pošle druhou, prostředí založené výhradně na kontejner instancí Azure nabízí nejrychlejší způsob začít a jak nakládat s vysoce proměnné úlohy.</span><span class="sxs-lookup"><span data-stu-id="fbcce-127">Because they start quickly and bill by the second, an environment based exclusively on Azure Container Instances offers the fastest way to get started and to deal with highly variable workloads.</span></span>

### <a name="combination-of-container-instances-and-containers-in-virtual-machines"></a><span data-ttu-id="fbcce-128">Kombinace instancí kontejnerů a kontejnery v virtuální počítače</span><span class="sxs-lookup"><span data-stu-id="fbcce-128">Combination of Container Instances and Containers in Virtual Machines</span></span>

<span data-ttu-id="fbcce-129">Pro dlouhodobé, stabilní úlohy Orchestrace kontejnery v clusteru s podporou vyhrazených virtuálních počítačů obvykle bude levnějších než spuštěný stejný kontejnery s instancí kontejnerů.</span><span class="sxs-lookup"><span data-stu-id="fbcce-129">For long-running, stable workloads, orchestrating containers in a cluster of dedicated virtual machines will typically be cheaper than running the same containers with Container Instances.</span></span> <span data-ttu-id="fbcce-130">Kontejner instancí však nabízí vynikající řešení pro rychlé rozšiřování a smluvní vaší celkovou kapacitu, jak nakládat s neočekávanou nebo krátkodobou špičky využití.</span><span class="sxs-lookup"><span data-stu-id="fbcce-130">However, Container Instances offer a great solution for quickly expanding and contracting your overall capacity to deal with unexpected or short-lived spikes in usage.</span></span> <span data-ttu-id="fbcce-131">Místo škálování počtu virtuálních počítačů v clusteru, pak nasazení další kontejnery na tyto počítače, orchestrator můžete jednoduše naplánovat další kontejnery pomocí kontejner instancí a odstranit, když jsou už potřeba.</span><span class="sxs-lookup"><span data-stu-id="fbcce-131">Rather than scaling out the number of virtual machines in your cluster, then deploying additional containers onto those machines, the orchestrator can simply schedule the additional containers using Container Instances and delete them when they're no longer needed.</span></span>

## <a name="sample-implementation-azure-container-instances-connector-for-kubernetes"></a><span data-ttu-id="fbcce-132">Ukázka implementace: Azure kontejner instancí konektor pro Kubernetes</span><span class="sxs-lookup"><span data-stu-id="fbcce-132">Sample implementation: Azure Container Instances Connector for Kubernetes</span></span>

<span data-ttu-id="fbcce-133">K předvedení, jak integrovat platformy orchestration kontejner s instancemi Azure kontejneru, jsme spustili sestavování [ukázka konektor pro Kubernetes][aci-connector-k8s].</span><span class="sxs-lookup"><span data-stu-id="fbcce-133">To demonstrate how container orchestration platforms can integrate with Azure Container Instances, we have started building a [sample connector for Kubernetes][aci-connector-k8s].</span></span> 

<span data-ttu-id="fbcce-134">Konektor pro Kubernetes napodobuje [kubelet] [ kubelet-doc] registrace jako uzel neomezená kapacitu a odeslání vytvoření [pracovními stanicemi soustředěnými kolem] [ pod-doc] kontejneru a skupiny v Azure kontejner instancí.</span><span class="sxs-lookup"><span data-stu-id="fbcce-134">The connector for Kubernetes mimics the [kubelet][kubelet-doc] by registering as a node with unlimited capacity and dispatching the creation of [pods][pod-doc] as container groups in Azure Container Instances.</span></span> 

<!-- ![ACI Connector for Kubernetes][aci-connector-k8s-gif] -->

<span data-ttu-id="fbcce-135">Konektory pro ostatní orchestrators může být postavená, podobně jako integrovat platformy primitiv kombinovat napájení nástroje orchestrator rozhraní API s rychlostí a zjednodušení správy kontejnerů v Azure kontejner instancí.</span><span class="sxs-lookup"><span data-stu-id="fbcce-135">Connectors for other orchestrators could be built that similarly integrated with platform primitives to combine the power of the orchestrator API with the speed and simplicity of managing containers in Azure Container Instances.</span></span>

> [!WARNING]
> <span data-ttu-id="fbcce-136">Konektor nástroje ACI pro Kubernetes *experimentální* a neměl by se používat v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="fbcce-136">The ACI connector for Kubernetes is *experimental* and should not be used in production.</span></span>

## <a name="next-steps"></a><span data-ttu-id="fbcce-137">Další kroky</span><span class="sxs-lookup"><span data-stu-id="fbcce-137">Next steps</span></span>

<span data-ttu-id="fbcce-138">Vytvoření vaší první kontejneru s instancí kontejnerů Azure pomocí [úvodní příručka](container-instances-quickstart.md).</span><span class="sxs-lookup"><span data-stu-id="fbcce-138">Create your first container with Azure Container Instances using the [quick start guide](container-instances-quickstart.md).</span></span>

<!-- IMAGES -->
[aci-connector-k8s-gif]: ./media/container-instances-orchestrator-relationship/aci-connector-k8s.gif

<!-- LINKS -->
[aci-connector-k8s]: https://github.com/azure/aci-connector-k8s
[kubelet-doc]: https://kubernetes.io/docs/admin/kubelet/
[pod-doc]: https://kubernetes.io/docs/concepts/workloads/pods/pod/