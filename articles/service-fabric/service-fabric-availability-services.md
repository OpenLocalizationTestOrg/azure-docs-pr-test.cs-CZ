---
title: "Dostupnost služeb Service Fabric | Microsoft Docs"
description: "Popisuje zjišťování chyb, převzetí služeb při selhání a obnovení pro služby"
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: 279ba4a4-f2ef-4e4e-b164-daefd10582e4
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 41ff2c3129facb0eea9d896ce75d7343ae2a018e
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="availability-of-service-fabric-services"></a><span data-ttu-id="c8d71-103">Dostupnost služeb Service Fabric</span><span class="sxs-lookup"><span data-stu-id="c8d71-103">Availability of Service Fabric services</span></span>
<span data-ttu-id="c8d71-104">Tento článek poskytuje přehled o tom, jak Service Fabric udržuje dostupnosti služby.</span><span class="sxs-lookup"><span data-stu-id="c8d71-104">This article gives an overview of how Service Fabric maintains availability of a service.</span></span>

## <a name="availability-of-service-fabric-stateless-services"></a><span data-ttu-id="c8d71-105">Dostupnost bezstavové služby Service Fabric</span><span class="sxs-lookup"><span data-stu-id="c8d71-105">Availability of Service Fabric stateless services</span></span>
<span data-ttu-id="c8d71-106">Služby Azure Service Fabric může být stavová nebo bezstavové.</span><span class="sxs-lookup"><span data-stu-id="c8d71-106">Azure Service Fabric services can be either stateful or stateless.</span></span> <span data-ttu-id="c8d71-107">Bezstavové služby je služba aplikace, která nemá žádné [lokálního stavu](service-fabric-concepts-state.md) které musí být vysoce k dispozici nebo není spolehlivá.</span><span class="sxs-lookup"><span data-stu-id="c8d71-107">A stateless service is an application service that does not have any [local state](service-fabric-concepts-state.md) that needs to be highly available or reliable.</span></span>

<span data-ttu-id="c8d71-108">Vytvoření bezstavové služby vyžaduje definování `InstanceCount`.</span><span class="sxs-lookup"><span data-stu-id="c8d71-108">Creating a stateless service requires defining an `InstanceCount`.</span></span> <span data-ttu-id="c8d71-109">Počet instancí definuje počet instancí bezstavové služby aplikační logiky, která by měla být spuštěná v clusteru.</span><span class="sxs-lookup"><span data-stu-id="c8d71-109">The instance count defines the number of instances of the stateless service's application logic that should be running in the cluster.</span></span> <span data-ttu-id="c8d71-110">Zvýšení počtu instancí je doporučený způsob škálování bezstavové služby.</span><span class="sxs-lookup"><span data-stu-id="c8d71-110">Increasing the number of instances is the recommended way of scaling out a stateless service.</span></span>

<span data-ttu-id="c8d71-111">Pokud instance bezstavové s názvem služby nezdaří, je vytvořena nová instance na některé oprávněné uzlu v clusteru.</span><span class="sxs-lookup"><span data-stu-id="c8d71-111">When an instance of a stateless named service fails, a new instance is created on some eligible node in the cluster.</span></span> <span data-ttu-id="c8d71-112">Například instance bezstavové služby může dojít k selhání na Uzel1 a znovu vytvořen na počítač Uzel5.</span><span class="sxs-lookup"><span data-stu-id="c8d71-112">For example, a stateless service instance might fail on Node1 and be recreated on Node5.</span></span>

## <a name="availability-of-service-fabric-stateful-services"></a><span data-ttu-id="c8d71-113">Dostupnost stavové služby Service Fabric</span><span class="sxs-lookup"><span data-stu-id="c8d71-113">Availability of Service Fabric stateful services</span></span>
<span data-ttu-id="c8d71-114">Stavová služba má některé stavu s ním spojená.</span><span class="sxs-lookup"><span data-stu-id="c8d71-114">A stateful service has some state associated with it.</span></span> <span data-ttu-id="c8d71-115">V Service Fabric stavové služby je modelovaná jako sadu replik.</span><span class="sxs-lookup"><span data-stu-id="c8d71-115">In Service Fabric, a stateful service is modeled as a set of replicas.</span></span> <span data-ttu-id="c8d71-116">Každá replika je spuštěné instance kódu služby, která má také kopii stavu pro danou službu.</span><span class="sxs-lookup"><span data-stu-id="c8d71-116">Each replica is a running instance of the code of the service that also has a copy of the state for that service.</span></span> <span data-ttu-id="c8d71-117">Operace čtení a zápisu jsou prováděny v jedné replice (označovaný jako primární).</span><span class="sxs-lookup"><span data-stu-id="c8d71-117">Read and write operations are performed at one replica (called the Primary).</span></span> <span data-ttu-id="c8d71-118">Změny stavu z zápisu operace jsou *replikovat* do jiných replik sady replik. (názvem aktivní sekundární databáze) a použít.</span><span class="sxs-lookup"><span data-stu-id="c8d71-118">Changes to state from write operations are *replicated* to the other replicas in the replica set (called Active Secondaries) and applied.</span></span> 

<span data-ttu-id="c8d71-119">Může existovat pouze jedna primární replika, ale může být více aktivní sekundární repliky.</span><span class="sxs-lookup"><span data-stu-id="c8d71-119">There can be only one Primary replica, but there can be multiple Active Secondary replicas.</span></span> <span data-ttu-id="c8d71-120">Počet aktivní sekundární repliky se dá konfigurovat, a vyšší počet replik tolerovat větší počet souběžných softwaru a selhání hardwaru.</span><span class="sxs-lookup"><span data-stu-id="c8d71-120">The number of Active Secondary replicas is configurable, and a higher number of replicas can tolerate a greater number of concurrent software and hardware failures.</span></span>

<span data-ttu-id="c8d71-121">Pokud se primární replika přestane fungovat, Service Fabric provede jeden aktivní sekundární repliky nový primární repliky.</span><span class="sxs-lookup"><span data-stu-id="c8d71-121">If the Primary replica goes down, Service Fabric makes one of the Active Secondary replicas the new Primary replica.</span></span> <span data-ttu-id="c8d71-122">Tato aktivní sekundární replika už má v aktualizovaném stavu (prostřednictvím *replikace*), a můžete pokračovat ve zpracovávání pro další čtení a zápisu operace.</span><span class="sxs-lookup"><span data-stu-id="c8d71-122">This Active Secondary replica already has the updated version of the state (via *replication*), and it can continue processing further read and write operations.</span></span>

<span data-ttu-id="c8d71-123">Tento koncept je buď primární, nebo aktivní sekundární repliky se označuje jako Role repliky.</span><span class="sxs-lookup"><span data-stu-id="c8d71-123">This concept, of a replica being either a Primary or Active Secondary, is known as the Replica Role.</span></span>

### <a name="replica-roles"></a><span data-ttu-id="c8d71-124">Role repliky</span><span class="sxs-lookup"><span data-stu-id="c8d71-124">Replica roles</span></span>
<span data-ttu-id="c8d71-125">Role repliky se používá ke správě životního cyklu stavu spravuje repliky.</span><span class="sxs-lookup"><span data-stu-id="c8d71-125">The role of a replica is used to manage the life cycle of the state being managed by that replica.</span></span> <span data-ttu-id="c8d71-126">Požadavků na čtení repliku, jejichž role je primární služby.</span><span class="sxs-lookup"><span data-stu-id="c8d71-126">A replica whose role is Primary services read requests.</span></span> <span data-ttu-id="c8d71-127">Primární také zpracovává všechny požadavky na zápis aktualizace stavu a replikovat změny.</span><span class="sxs-lookup"><span data-stu-id="c8d71-127">The Primary also handles all write requests by updating its state and replicating the changes.</span></span> <span data-ttu-id="c8d71-128">Tyto změny se použijí k aktivní sekundární databáze v replikách.</span><span class="sxs-lookup"><span data-stu-id="c8d71-128">These changes are applied to the Active Secondaries in the replica set.</span></span> <span data-ttu-id="c8d71-129">Úloha aktivní sekundární je přijímat změny stavu, které primární repliky replikována a aktualizujte jeho zobrazení stavu.</span><span class="sxs-lookup"><span data-stu-id="c8d71-129">The job of an Active Secondary is to receive state changes that the Primary replica has replicated and update its view of the state.</span></span>

> [!NOTE]
> <span data-ttu-id="c8d71-130">Vyšší úrovně programování modelů, jako [Reliable Actors](service-fabric-reliable-actors-introduction.md) a [spolehlivé služby](service-fabric-reliable-services-introduction.md) skrýt koncept role repliky od vývojáře.</span><span class="sxs-lookup"><span data-stu-id="c8d71-130">Higher-level programming models such as [Reliable Actors](service-fabric-reliable-actors-introduction.md) and [Reliable Services](service-fabric-reliable-services-introduction.md) hide the concept of replica role from the developer.</span></span> <span data-ttu-id="c8d71-131">V aktéři je zbytečné, při služby je z velké části jednodušší pro většinu scénářů představu o roli.</span><span class="sxs-lookup"><span data-stu-id="c8d71-131">In Actors, the notion of role is unnecessary, while in Services it is largely simplified for most scenarios.</span></span>
>

## <a name="next-steps"></a><span data-ttu-id="c8d71-132">Další kroky</span><span class="sxs-lookup"><span data-stu-id="c8d71-132">Next steps</span></span>
<span data-ttu-id="c8d71-133">Další informace o konceptech Service Fabric najdete v následujících článcích:</span><span class="sxs-lookup"><span data-stu-id="c8d71-133">For more information on Service Fabric concepts, see the following articles:</span></span>

- [<span data-ttu-id="c8d71-134">Škálování služby Service Fabric</span><span class="sxs-lookup"><span data-stu-id="c8d71-134">Scaling Service Fabric services</span></span>](service-fabric-concepts-scalability.md)
- [<span data-ttu-id="c8d71-135">Vytváření oddílů služby Service Fabric</span><span class="sxs-lookup"><span data-stu-id="c8d71-135">Partitioning Service Fabric services</span></span>](service-fabric-concepts-partitioning.md)
- [<span data-ttu-id="c8d71-136">Definování a správu stavu</span><span class="sxs-lookup"><span data-stu-id="c8d71-136">Defining and managing state</span></span>](service-fabric-concepts-state.md)
- [<span data-ttu-id="c8d71-137">Reliable Services</span><span class="sxs-lookup"><span data-stu-id="c8d71-137">Reliable Services</span></span>](service-fabric-reliable-services-introduction.md)
