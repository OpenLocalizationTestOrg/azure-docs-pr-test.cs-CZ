---
title: "Monitorování výkonu služby infrastruktury aaaAzure | Microsoft Docs"
description: "Další informace o čítače výkonu pro monitorování a Diagnostika Azure Service Fabric clusterů."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/30/2017
ms.author: dekapur
ms.openlocfilehash: 54d4c62b7250a1f70b0898ba07ae5a37716f4cf4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="performance-metrics"></a><span data-ttu-id="abff2-103">Metriky výkonu</span><span class="sxs-lookup"><span data-stu-id="abff2-103">Performance metrics</span></span>

<span data-ttu-id="abff2-104">Metriky by měl být výkonu shromážděných toounderstand hello clusteru a také hello aplikace běžící v ní.</span><span class="sxs-lookup"><span data-stu-id="abff2-104">Metrics should be collected toounderstand hello performance of your cluster as well as hello applications running in it.</span></span> <span data-ttu-id="abff2-105">U clusterů Service Fabric doporučujeme shromažďování hello následující čítače výkonu.</span><span class="sxs-lookup"><span data-stu-id="abff2-105">For Service Fabric clusters, we recommend collecting hello following performance counters.</span></span>

## <a name="nodes"></a><span data-ttu-id="abff2-106">Uzly</span><span class="sxs-lookup"><span data-stu-id="abff2-106">Nodes</span></span>

<span data-ttu-id="abff2-107">Pro hello počítače v clusteru vezměte v úvahu shromažďování hello následující čítače výkonu toobetter pochopit hello zatížení na každém počítači a proveďte příslušné clusteru škálování rozhodnutí.</span><span class="sxs-lookup"><span data-stu-id="abff2-107">For hello machines in your cluster, consider collecting hello following performance counters toobetter understand hello load on each machine and make appropriate cluster scaling decisions.</span></span>

| <span data-ttu-id="abff2-108">Kategorie čítače</span><span class="sxs-lookup"><span data-stu-id="abff2-108">Counter Category</span></span> | <span data-ttu-id="abff2-109">Název čítače</span><span class="sxs-lookup"><span data-stu-id="abff2-109">Counter Name</span></span> |
| --- | --- |
| <span data-ttu-id="abff2-110">Fyzický disk (na disku)</span><span class="sxs-lookup"><span data-stu-id="abff2-110">PhysicalDisk(per Disk)</span></span> | <span data-ttu-id="abff2-111">Střední Délka fronty disku pro čtení</span><span class="sxs-lookup"><span data-stu-id="abff2-111">Avg. Disk Read Queue Length</span></span> |
| <span data-ttu-id="abff2-112">Fyzický disk (na disku)</span><span class="sxs-lookup"><span data-stu-id="abff2-112">PhysicalDisk(per Disk)</span></span> | <span data-ttu-id="abff2-113">Střední Délka fronty disku zápisu</span><span class="sxs-lookup"><span data-stu-id="abff2-113">Avg. Disk Write Queue Length</span></span> |
| <span data-ttu-id="abff2-114">Fyzický disk (na disku)</span><span class="sxs-lookup"><span data-stu-id="abff2-114">PhysicalDisk(per Disk)</span></span> | <span data-ttu-id="abff2-115">Střední Doba disku/čtení</span><span class="sxs-lookup"><span data-stu-id="abff2-115">Avg. Disk sec/Read</span></span> |
| <span data-ttu-id="abff2-116">Fyzický disk (na disku)</span><span class="sxs-lookup"><span data-stu-id="abff2-116">PhysicalDisk(per Disk)</span></span> | <span data-ttu-id="abff2-117">Střední Doba disku/zápis</span><span class="sxs-lookup"><span data-stu-id="abff2-117">Avg. Disk sec/Write</span></span> |
| <span data-ttu-id="abff2-118">Fyzický disk (na disku)</span><span class="sxs-lookup"><span data-stu-id="abff2-118">PhysicalDisk(per Disk)</span></span> | <span data-ttu-id="abff2-119">Čtení disku/s</span><span class="sxs-lookup"><span data-stu-id="abff2-119">Disk Reads/sec</span></span> |
| <span data-ttu-id="abff2-120">Fyzický disk (na disku)</span><span class="sxs-lookup"><span data-stu-id="abff2-120">PhysicalDisk(per Disk)</span></span> | <span data-ttu-id="abff2-121">Čtení z disku bajtů/s</span><span class="sxs-lookup"><span data-stu-id="abff2-121">Disk Read Bytes/sec</span></span> |
| <span data-ttu-id="abff2-122">Fyzický disk (na disku)</span><span class="sxs-lookup"><span data-stu-id="abff2-122">PhysicalDisk(per Disk)</span></span> | <span data-ttu-id="abff2-123">Zápis disku/s</span><span class="sxs-lookup"><span data-stu-id="abff2-123">Disk Writes/sec</span></span> |
| <span data-ttu-id="abff2-124">Fyzický disk (na disku)</span><span class="sxs-lookup"><span data-stu-id="abff2-124">PhysicalDisk(per Disk)</span></span> | <span data-ttu-id="abff2-125">Bajty zapisování na disk/s</span><span class="sxs-lookup"><span data-stu-id="abff2-125">Disk Write Bytes/sec</span></span> |
| <span data-ttu-id="abff2-126">Memory (Paměť)</span><span class="sxs-lookup"><span data-stu-id="abff2-126">Memory</span></span> | <span data-ttu-id="abff2-127">Počet MB k dispozici</span><span class="sxs-lookup"><span data-stu-id="abff2-127">Available MBytes</span></span> |
| <span data-ttu-id="abff2-128">Soubor stránkování</span><span class="sxs-lookup"><span data-stu-id="abff2-128">PagingFile</span></span> | <span data-ttu-id="abff2-129">% Využití</span><span class="sxs-lookup"><span data-stu-id="abff2-129">% Usage</span></span> |
| <span data-ttu-id="abff2-130">Process(Total)</span><span class="sxs-lookup"><span data-stu-id="abff2-130">Process(Total)</span></span> | <span data-ttu-id="abff2-131">% Času procesoru</span><span class="sxs-lookup"><span data-stu-id="abff2-131">% Processor Time</span></span> |
| <span data-ttu-id="abff2-132">Proces (pro službu)</span><span class="sxs-lookup"><span data-stu-id="abff2-132">Process (per service)</span></span> | <span data-ttu-id="abff2-133">% Času procesoru</span><span class="sxs-lookup"><span data-stu-id="abff2-133">% Processor Time</span></span> |
| <span data-ttu-id="abff2-134">Proces (pro službu)</span><span class="sxs-lookup"><span data-stu-id="abff2-134">Process (per service)</span></span> | <span data-ttu-id="abff2-135">ID procesu</span><span class="sxs-lookup"><span data-stu-id="abff2-135">ID Process</span></span> |
| <span data-ttu-id="abff2-136">Proces (pro službu)</span><span class="sxs-lookup"><span data-stu-id="abff2-136">Process (per service)</span></span> | <span data-ttu-id="abff2-137">Soukromé bajty</span><span class="sxs-lookup"><span data-stu-id="abff2-137">Private Bytes</span></span> |
| <span data-ttu-id="abff2-138">Proces (pro službu)</span><span class="sxs-lookup"><span data-stu-id="abff2-138">Process (per service)</span></span> | <span data-ttu-id="abff2-139">Počet vláken</span><span class="sxs-lookup"><span data-stu-id="abff2-139">Thread Count</span></span> |
| <span data-ttu-id="abff2-140">Proces (pro službu)</span><span class="sxs-lookup"><span data-stu-id="abff2-140">Process (per service)</span></span> | <span data-ttu-id="abff2-141">Virtuální bajty</span><span class="sxs-lookup"><span data-stu-id="abff2-141">Virtual Bytes</span></span> |
| <span data-ttu-id="abff2-142">Proces (pro službu)</span><span class="sxs-lookup"><span data-stu-id="abff2-142">Process (per service)</span></span> | <span data-ttu-id="abff2-143">Pracovní sada</span><span class="sxs-lookup"><span data-stu-id="abff2-143">Working Set</span></span> |
| <span data-ttu-id="abff2-144">Proces (pro službu)</span><span class="sxs-lookup"><span data-stu-id="abff2-144">Process (per service)</span></span> | <span data-ttu-id="abff2-145">Pracovní sada – privátní</span><span class="sxs-lookup"><span data-stu-id="abff2-145">Working Set - Private</span></span> |

## <a name="net-applications-and-services"></a><span data-ttu-id="abff2-146">Aplikace .NET a služby</span><span class="sxs-lookup"><span data-stu-id="abff2-146">.NET applications and services</span></span>

<span data-ttu-id="abff2-147">Shromažďujte následující čítače, pokud provádíte nasazení clusteru tooyour služby .NET hello.</span><span class="sxs-lookup"><span data-stu-id="abff2-147">Collect hello following counters if you are deploying .NET services tooyour cluster.</span></span> 

| <span data-ttu-id="abff2-148">Kategorie čítače</span><span class="sxs-lookup"><span data-stu-id="abff2-148">Counter Category</span></span> | <span data-ttu-id="abff2-149">Název čítače</span><span class="sxs-lookup"><span data-stu-id="abff2-149">Counter Name</span></span> |
| --- | --- |
| <span data-ttu-id="abff2-150">Využívání paměti rozhraním .NET CLR (pro službu)</span><span class="sxs-lookup"><span data-stu-id="abff2-150">.NET CLR Memory (per service)</span></span> | <span data-ttu-id="abff2-151">ID procesu</span><span class="sxs-lookup"><span data-stu-id="abff2-151">Process ID</span></span> |
| <span data-ttu-id="abff2-152">Využívání paměti rozhraním .NET CLR (pro službu)</span><span class="sxs-lookup"><span data-stu-id="abff2-152">.NET CLR Memory (per service)</span></span> | <span data-ttu-id="abff2-153"># Celkový počet potvrzených bajtů</span><span class="sxs-lookup"><span data-stu-id="abff2-153"># Total committed Bytes</span></span> |
| <span data-ttu-id="abff2-154">Využívání paměti rozhraním .NET CLR (pro službu)</span><span class="sxs-lookup"><span data-stu-id="abff2-154">.NET CLR Memory (per service)</span></span> | <span data-ttu-id="abff2-155"># Celkový počet vyhrazené bajtů</span><span class="sxs-lookup"><span data-stu-id="abff2-155"># Total reserved Bytes</span></span> |
| <span data-ttu-id="abff2-156">Využívání paměti rozhraním .NET CLR (pro službu)</span><span class="sxs-lookup"><span data-stu-id="abff2-156">.NET CLR Memory (per service)</span></span> | <span data-ttu-id="abff2-157">Počet bajtů ve všech haldách</span><span class="sxs-lookup"><span data-stu-id="abff2-157"># Bytes in all Heaps</span></span> |
| <span data-ttu-id="abff2-158">Využívání paměti rozhraním .NET CLR (pro službu)</span><span class="sxs-lookup"><span data-stu-id="abff2-158">.NET CLR Memory (per service)</span></span> | <span data-ttu-id="abff2-159">Počet úklidů 0</span><span class="sxs-lookup"><span data-stu-id="abff2-159"># Gen 0 Collections</span></span> |
| <span data-ttu-id="abff2-160">Využívání paměti rozhraním .NET CLR (pro službu)</span><span class="sxs-lookup"><span data-stu-id="abff2-160">.NET CLR Memory (per service)</span></span> | <span data-ttu-id="abff2-161"># 1. generace kolekce</span><span class="sxs-lookup"><span data-stu-id="abff2-161"># Gen 1 Collections</span></span> |
| <span data-ttu-id="abff2-162">Využívání paměti rozhraním .NET CLR (pro službu)</span><span class="sxs-lookup"><span data-stu-id="abff2-162">.NET CLR Memory (per service)</span></span> | <span data-ttu-id="abff2-163"># Úklidů 2. generace</span><span class="sxs-lookup"><span data-stu-id="abff2-163"># Gen 2 Collections</span></span> |
| <span data-ttu-id="abff2-164">Využívání paměti rozhraním .NET CLR (pro službu)</span><span class="sxs-lookup"><span data-stu-id="abff2-164">.NET CLR Memory (per service)</span></span> | <span data-ttu-id="abff2-165">Čas</span><span class="sxs-lookup"><span data-stu-id="abff2-165">% Time in GC</span></span> |

### <a name="service-fabrics-custom-performance-counters"></a><span data-ttu-id="abff2-166">Čítače výkonu vlastní Service Fabric</span><span class="sxs-lookup"><span data-stu-id="abff2-166">Service Fabric's custom performance counters</span></span>

<span data-ttu-id="abff2-167">Service Fabric generuje vyžadovat značné množství vlastní čítače výkonu.</span><span class="sxs-lookup"><span data-stu-id="abff2-167">Service Fabric generates a substantial amount of custom performance counters.</span></span> <span data-ttu-id="abff2-168">Pokud máte hello SDK nainstalovat, v aplikaci sledování výkonu uvidíte kompletní seznam hello na počítač se systémem Windows (Start > Sledování výkonu).</span><span class="sxs-lookup"><span data-stu-id="abff2-168">If you have hello SDK installed, you can see hello comprehensive list on your Windows machine in your Performance Monitor application (Start > Performance Monitor).</span></span> 

<span data-ttu-id="abff2-169">V aplikacích hello nasazujete tooyour clusteru, pokud používáte Reliable Actors, přidejte countes z `Service Fabric Actor` a `Service Fabric Actor Method` kategorie (viz [Service Fabric spolehlivé aktéři diagnostiky](service-fabric-reliable-actors-diagnostics.md)).</span><span class="sxs-lookup"><span data-stu-id="abff2-169">In hello applications you are deploying tooyour cluster, if you are using Reliable Actors, add countes from `Service Fabric Actor` and `Service Fabric Actor Method` categories (see [Service Fabric Reliable Actors Diagnostics](service-fabric-reliable-actors-diagnostics.md)).</span></span>

<span data-ttu-id="abff2-170">Pokud používáte spolehlivé služby, podobně jako máme `Service Fabric Service` a `Service Fabric Service Method` kategorie čítače, které mají shromažďovat čítače z.</span><span class="sxs-lookup"><span data-stu-id="abff2-170">If you use Reliable Services, we similarly have `Service Fabric Service` and `Service Fabric Service Method` counter categories that you should collect counters from.</span></span> 

<span data-ttu-id="abff2-171">Pokud používáte spolehlivé kolekcí, doporučujeme přidání hello `Avg. Transaction ms/Commit` z hello `Service Fabric Transactional Replicator` toocollect hello průměrná potvrzení latence za metrika transakce.</span><span class="sxs-lookup"><span data-stu-id="abff2-171">If you use Reliable Collections, we recommend adding hello `Avg. Transaction ms/Commit` from hello `Service Fabric Transactional Replicator` toocollect hello average commit latency per transaction metric.</span></span>


## <a name="next-steps"></a><span data-ttu-id="abff2-172">Další kroky</span><span class="sxs-lookup"><span data-stu-id="abff2-172">Next steps</span></span>

* <span data-ttu-id="abff2-173">Další informace o [generování událostí na úrovni platformy hello](service-fabric-diagnostics-event-generation-infra.md) v Service Fabric</span><span class="sxs-lookup"><span data-stu-id="abff2-173">Learn more about [event generation at hello platform level](service-fabric-diagnostics-event-generation-infra.md) in Service Fabric</span></span>
* <span data-ttu-id="abff2-174">Shromažďování metrik výkonu prostřednictvím [Azure Diagnostics](service-fabric-diagnostics-event-aggregation-wad.md)</span><span class="sxs-lookup"><span data-stu-id="abff2-174">Collect performance metrics through [Azure Diagnostics](service-fabric-diagnostics-event-aggregation-wad.md)</span></span>
