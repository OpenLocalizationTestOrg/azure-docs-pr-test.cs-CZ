---
title: "Vytvořit clustery se Azure Service Fabric na serveru Windows a Linux | Microsoft Docs"
description: "Spusťte na serveru Windows a Linux, tzn., budete moci nasadit a hostování aplikací Service Fabric kdekoli clusterů Service Fabric můžete spustit systém Windows Server nebo Linux."
services: service-fabric
documentationcenter: .net
author: Chackdan
manager: timlt
editor: 
ms.assetid: 19ca51e8-69b9-4952-b4b5-4bf04cded217
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/08/2017
ms.author: chackdan
ms.openlocfilehash: dcc7c088d7b6db7af334977315f122dca3c17f69
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="create-service-fabric-clusters-on-windows-server-or-linux"></a><span data-ttu-id="11bdb-103">Vytvoření clusterů Service Fabric na Windows Server nebo Linux</span><span class="sxs-lookup"><span data-stu-id="11bdb-103">Create Service Fabric clusters on Windows Server or Linux</span></span>
<span data-ttu-id="11bdb-104">Cluster služby Azure Service Fabric je sada virtuální nebo fyzické počítače, do kterých jsou nasazené a spravovat vaše mikroslužeb připojené k síti.</span><span class="sxs-lookup"><span data-stu-id="11bdb-104">An Azure Service Fabric cluster is a network-connected set of virtual or physical machines into which your microservices are deployed and managed.</span></span> <span data-ttu-id="11bdb-105">Počítač nebo virtuální počítač, který je součástí clusteru, se nazývá uzlem clusteru.</span><span class="sxs-lookup"><span data-stu-id="11bdb-105">A machine or VM that is part of a cluster is called a cluster node.</span></span> <span data-ttu-id="11bdb-106">Clustery můžete škálovat na tisících uzlů.</span><span class="sxs-lookup"><span data-stu-id="11bdb-106">Clusters can scale to thousands of nodes.</span></span> <span data-ttu-id="11bdb-107">Pokud přidáte nové uzly do clusteru, Service Fabric znovu vytvoří rovnováhu repliky oddílu služby a instance napříč vyšší počet uzlů.</span><span class="sxs-lookup"><span data-stu-id="11bdb-107">If you add new nodes to the cluster, Service Fabric rebalances the service partition replicas and instances across the increased number of nodes.</span></span> <span data-ttu-id="11bdb-108">Celkové zlepšuje výkon aplikace a snižuje kolize pro přístup do paměti.</span><span class="sxs-lookup"><span data-stu-id="11bdb-108">Overall application performance improves and contention for access to memory decreases.</span></span> <span data-ttu-id="11bdb-109">Pokud uzly v clusteru nejsou používány efektivně, můžete snížit počet uzlů v clusteru.</span><span class="sxs-lookup"><span data-stu-id="11bdb-109">If the nodes in the cluster are not being used efficiently, you can decrease the number of nodes in the cluster.</span></span> <span data-ttu-id="11bdb-110">Service Fabric opakujte znovu vytvoří rovnováhu repliky oddílu a instance napříč ke snížení počtu uzlů zajistit lepší využití hardwaru na každém uzlu.</span><span class="sxs-lookup"><span data-stu-id="11bdb-110">Service Fabric again rebalances the partition replicas and instances across the decreased number of nodes to make better use of the hardware on each node.</span></span>

<span data-ttu-id="11bdb-111">Service Fabric umožňuje vytváření clusterů Service Fabric na všechny virtuální počítače nebo počítače se systémem Windows Server nebo Linux.</span><span class="sxs-lookup"><span data-stu-id="11bdb-111">Service Fabric allows for the creation of Service Fabric clusters on any VMs or computers running Windows Server or Linux.</span></span> <span data-ttu-id="11bdb-112">To znamená, že budete moci nasadit a spouštět aplikace Service Fabric v jakémkoli prostředí, kde máte sadu Windows Server nebo Linux počítače, které jsou vzájemně provázány, je jej na místních počítačích Microsoft Azure nebo kteréhokoli poskytovatele cloudových služeb.</span><span class="sxs-lookup"><span data-stu-id="11bdb-112">This means you are able to deploy and run Service Fabric applications in any environment where you have a set of Windows Server or Linux computers that are interconnected, be it on-premises, Microsoft Azure or with any cloud provider.</span></span>

## <a name="create-service-fabric-clusters-on-azure"></a><span data-ttu-id="11bdb-113">Vytvoření clusterů Service Fabric v Azure</span><span class="sxs-lookup"><span data-stu-id="11bdb-113">Create Service Fabric clusters on Azure</span></span>
<span data-ttu-id="11bdb-114">Vytvoření clusteru v Azure se provádí prostřednictvím šablonu Model prostředků nebo portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="11bdb-114">Creating a cluster on Azure is done either via a Resource Model template or the Azure portal.</span></span> <span data-ttu-id="11bdb-115">Čtení [vytvořit cluster Service Fabric pomocí šablony Resource Manageru](service-fabric-cluster-creation-via-arm.md) nebo [z portálu Azure vytvořit cluster Service Fabric](service-fabric-cluster-creation-via-portal.md) Další informace.</span><span class="sxs-lookup"><span data-stu-id="11bdb-115">Read [Create a Service Fabric cluster by using a Resource Manager template](service-fabric-cluster-creation-via-arm.md) or [Create a Service Fabric cluster from the Azure portal](service-fabric-cluster-creation-via-portal.md) for more information.</span></span>

## <a name="supported-operating-systems-for-clusters-on-azure"></a><span data-ttu-id="11bdb-116">Podporované operační systémy pro clustery v Azure</span><span class="sxs-lookup"><span data-stu-id="11bdb-116">Supported operating systems for clusters on Azure</span></span>
<span data-ttu-id="11bdb-117">Budete moci vytvořit clustery se na virtuálních počítačích s těmito operačními systémy:</span><span class="sxs-lookup"><span data-stu-id="11bdb-117">You are able to create clusters on VMs running these operating systems:</span></span>

* <span data-ttu-id="11bdb-118">Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="11bdb-118">Windows Server 2012 R2</span></span>
* <span data-ttu-id="11bdb-119">Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="11bdb-119">Windows Server 2016</span></span> 
* <span data-ttu-id="11bdb-120">16.04 Ubuntu Linux (ve verzi public preview)</span><span class="sxs-lookup"><span data-stu-id="11bdb-120">Linux Ubuntu 16.04 (in public preview)</span></span> 

## <a name="create-service-fabric-standalone-clusters-on-premises-or-with-any-cloud-provider"></a><span data-ttu-id="11bdb-121">Vytvořit samostatný Service Fabric clustery v místě nebo kteréhokoli poskytovatele cloudových služeb</span><span class="sxs-lookup"><span data-stu-id="11bdb-121">Create Service Fabric standalone clusters on-premises or with any cloud provider</span></span>
<span data-ttu-id="11bdb-122">Service Fabric nabízí balíček instalace vytvořit samostatnou Service Fabric clustery místně nebo na všechny poskytovatele cloudových služeb</span><span class="sxs-lookup"><span data-stu-id="11bdb-122">Service Fabric provides an install package for you to create standalone Service Fabric clusters on-premises or on any cloud provider</span></span>

<span data-ttu-id="11bdb-123">Další informace o nastavení infrastruktury služby samostatné clustery v systému Windows Server, přečtěte si [vytváření clusteru Service Fabric pro systém Windows Server](service-fabric-cluster-creation-for-windows-server.md)</span><span class="sxs-lookup"><span data-stu-id="11bdb-123">For more information on setting up standalone service fabric clusters on Windows Server, read [Service Fabric cluster creation for Windows Server](service-fabric-cluster-creation-for-windows-server.md)</span></span>

### <a name="any-cloud-deployments-vs-on-premises-deployments"></a><span data-ttu-id="11bdb-124">Všechna nasazení cloudu a místních nasazení</span><span class="sxs-lookup"><span data-stu-id="11bdb-124">Any cloud deployments vs. on-premises deployments</span></span>
<span data-ttu-id="11bdb-125">Proces vytvoření místní cluster Service Fabric je podobný procesu vytváření clusteru s podporou na všechny cloudové zvoleného sadu virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="11bdb-125">The process for creating a Service Fabric cluster on-premises is similar to the process of creating a cluster on any cloud of your choice with a set of VMs.</span></span> <span data-ttu-id="11bdb-126">Počáteční kroky ke zřízení virtuálních počítačů se řídí poskytovatele cloudové nebo místní prostředí, který používáte.</span><span class="sxs-lookup"><span data-stu-id="11bdb-126">The initial steps to provision the VMs are governed by the cloud provider or on-premises environment that you are using.</span></span> <span data-ttu-id="11bdb-127">Jakmile máte sadu virtuálních počítačů s připojením k síti povolena mezi nimi, pak postup nastavte balíček Service Fabric, upravit nastavení clusteru a spusťte vytvoření clusteru a správy skriptů jsou identické.</span><span class="sxs-lookup"><span data-stu-id="11bdb-127">Once you have a set of VMs with network connectivity enabled between them, then the steps to set up the Service Fabric package, edit the cluster settings, and run the cluster creation and management scripts are identical.</span></span> <span data-ttu-id="11bdb-128">To zajišťuje, že znalosti a zkušenosti provoz a správu clusterů Service Fabric převoditelné když zvolíte cíle nové hostitelských prostředích.</span><span class="sxs-lookup"><span data-stu-id="11bdb-128">This ensures that your knowledge and experience of operating and managing Service Fabric clusters is transferable when you choose to target new hosting environments.</span></span>

### <a name="benefits-of-creating-standalone-service-fabric-clusters"></a><span data-ttu-id="11bdb-129">Výhody vytvoření samostatných clusterů Service Fabric</span><span class="sxs-lookup"><span data-stu-id="11bdb-129">Benefits of creating standalone Service Fabric clusters</span></span>
* <span data-ttu-id="11bdb-130">Jste volné vyberte všechny poskytovatele cloudu k hostování clusteru.</span><span class="sxs-lookup"><span data-stu-id="11bdb-130">You are free to choose any cloud provider to host your cluster.</span></span>
* <span data-ttu-id="11bdb-131">Aplikace Service Fabric, jednou napsané, lze spustit v prostředí s více hostitelských s minimální a žádné změny.</span><span class="sxs-lookup"><span data-stu-id="11bdb-131">Service Fabric applications, once written, can be run in multiple hosting environments with minimal to no changes.</span></span>
* <span data-ttu-id="11bdb-132">Informace o vytváření aplikací Service Fabric představuje z jednoho hostování prostředí do druhého.</span><span class="sxs-lookup"><span data-stu-id="11bdb-132">Knowledge of building Service Fabric applications carries over from one hosting environment to another.</span></span>
* <span data-ttu-id="11bdb-133">Provozní prostředí, jak spouštět a spravovat Service Fabric clusterů má u sebe přes z jednoho prostředí do druhého.</span><span class="sxs-lookup"><span data-stu-id="11bdb-133">Operational experience of running and managing Service Fabric clusters carries over from one environment to another.</span></span>
* <span data-ttu-id="11bdb-134">Široká zákazníka reach je bez vazby hostování prostředí omezení.</span><span class="sxs-lookup"><span data-stu-id="11bdb-134">Broad customer reach is unbounded by hosting environment constraints.</span></span>
* <span data-ttu-id="11bdb-135">Další úroveň spolehlivosti a ochranu proti výpadkům rozšířeným existuje, protože můžete přesunout službu do jiného prostředí nasazení Pokud zprostředkovatele dat centra nebo cloudu přerušení spojení.</span><span class="sxs-lookup"><span data-stu-id="11bdb-135">An extra layer of reliability and protection against widespread outages exists because you can move the services over to another deployment environment if a data center or cloud provider has a blackout.</span></span>

## <a name="supported-operating-systems-for-standalone-clusters"></a><span data-ttu-id="11bdb-136">Podporované operační systémy pro samostatné clustery</span><span class="sxs-lookup"><span data-stu-id="11bdb-136">Supported operating systems for standalone clusters</span></span>
<span data-ttu-id="11bdb-137">Budete moci vytvořit clustery se na virtuální počítače nebo počítačů s těmito operačními systémy:</span><span class="sxs-lookup"><span data-stu-id="11bdb-137">You are able to create clusters on VMs or computers running these operating systems:</span></span>

* <span data-ttu-id="11bdb-138">Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="11bdb-138">Windows Server 2012 R2</span></span>
* <span data-ttu-id="11bdb-139">Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="11bdb-139">Windows Server 2016</span></span> 
* <span data-ttu-id="11bdb-140">Linux (už brzy)</span><span class="sxs-lookup"><span data-stu-id="11bdb-140">Linux ( coming soon)</span></span>

## <a name="advantages-of-service-fabric-clusters-on-azure-over-standalone-service-fabric-clusters-created-on-premises"></a><span data-ttu-id="11bdb-141">Vytvořit místní výhod clusterů Service Fabric v Azure přes samostatnou bitovou clusterů Service Fabric</span><span class="sxs-lookup"><span data-stu-id="11bdb-141">Advantages of Service Fabric clusters on Azure over standalone Service Fabric clusters created on-premises</span></span>
<span data-ttu-id="11bdb-142">Clusterů Service Fabric systémem Azure poskytuje možnost výhod oproti místní, takže pokud nemáte konkrétní potřeby kterém spouštíte clustery, pak doporučujeme vám spustit v Azure.</span><span class="sxs-lookup"><span data-stu-id="11bdb-142">Running Service Fabric clusters on Azure provides advantages over the on-premises option, so if you don't have specific needs for where you run your clusters, then we suggest that you run them on Azure.</span></span> <span data-ttu-id="11bdb-143">V Azure poskytujeme integrace s dalšími funkce Azure a službami, které usnadňuje operace a Správa clusteru jednodušší a spolehlivější.</span><span class="sxs-lookup"><span data-stu-id="11bdb-143">On Azure, we provide integration with other Azure features and services, which makes operations and management of the cluster easier and more reliable.</span></span>

* <span data-ttu-id="11bdb-144">**Portál Azure:** portál Azure umožňuje snadno vytvářet a spravovat clustery.</span><span class="sxs-lookup"><span data-stu-id="11bdb-144">**Azure portal:** Azure portal makes it easy to create and manage clusters.</span></span>
* <span data-ttu-id="11bdb-145">**Azure Resource Manager:** pomocí nástroje Azure Resource Manager umožňuje snadnou správu všechny prostředky používané clusteru jako jednotku a zjednodušuje sledování nákladů a fakturace.</span><span class="sxs-lookup"><span data-stu-id="11bdb-145">**Azure Resource Manager:** Use of Azure Resource Manager allows easy management of all resources used by the cluster as a unit and simplifies cost tracking and billing.</span></span>
* <span data-ttu-id="11bdb-146">**Cluster Service Fabric jako prostředek Azure** cluster A Service Fabric je prostředek ARM, abyste mohli ho stejně jako ostatní prostředky ARM v Azure.</span><span class="sxs-lookup"><span data-stu-id="11bdb-146">**Service Fabric Cluster as an Azure Resource** A Service Fabric cluster is an ARM resource, so you can model it like you do other ARM resources in Azure.</span></span>
* <span data-ttu-id="11bdb-147">**Integrace s infrastruktury Azure** Service Fabric koordinuje s základní infrastrukturu Azure pro operační systém, sítě a jiných upgrady ke zlepšení dostupnosti a spolehlivosti aplikací.</span><span class="sxs-lookup"><span data-stu-id="11bdb-147">**Integration with Azure Infrastructure** Service Fabric coordinates with the underlying Azure infrastructure for OS, network, and other upgrades to improve availability and reliability of your applications.</span></span>  
* <span data-ttu-id="11bdb-148">**Diagnostika:** v Azure, poskytujeme integraci s Azure diagnostiku a analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="11bdb-148">**Diagnostics:** On Azure, we provide integration with Azure diagnostics and Log Analytics.</span></span>
* <span data-ttu-id="11bdb-149">**Automatické škálování:** pro clustery v Azure, poskytujeme integrovanou funkci automatického škálování z důvodu sady škálování virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="11bdb-149">**Auto-scaling:** For clusters on Azure, we provide built-in auto-scaling functionality due to Virtual Machine scale-sets.</span></span> <span data-ttu-id="11bdb-150">V jiných prostředích cloudu a místní budete muset vytvořit vlastní automatické škálování funkce nebo škálování ručně pomocí rozhraní API, která zveřejňuje Service Fabric pro škálování clusterů.</span><span class="sxs-lookup"><span data-stu-id="11bdb-150">In on-premises and other cloud environments, you have to build your own auto-scaling feature or scale manually using the APIs that Service Fabric exposes for scaling clusters.</span></span>

## <a name="next-steps"></a><span data-ttu-id="11bdb-151">Další kroky</span><span class="sxs-lookup"><span data-stu-id="11bdb-151">Next steps</span></span>

* <span data-ttu-id="11bdb-152">Vytvoření clusteru s podporou na virtuální počítače nebo počítače se systémem Windows Server: [vytváření clusteru Service Fabric pro systém Windows Server](service-fabric-cluster-creation-for-windows-server.md)</span><span class="sxs-lookup"><span data-stu-id="11bdb-152">Create a cluster on VMs or computers running Windows Server: [Service Fabric cluster creation for Windows Server](service-fabric-cluster-creation-for-windows-server.md)</span></span>
* <span data-ttu-id="11bdb-153">Vytvoření clusteru s podporou na virtuální počítače nebo počítače se systémem Linux: [Service Fabric v systému Linux](service-fabric-linux-overview.md)</span><span class="sxs-lookup"><span data-stu-id="11bdb-153">Create a cluster on VMs or computers running Linux: [Service Fabric on Linux](service-fabric-linux-overview.md)</span></span>
* <span data-ttu-id="11bdb-154">Informace o [možnostech podpory pro Service Fabric](service-fabric-support.md)</span><span class="sxs-lookup"><span data-stu-id="11bdb-154">Learn about [Service Fabric support options](service-fabric-support.md)</span></span>
