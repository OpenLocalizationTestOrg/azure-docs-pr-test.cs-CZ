---
title: "Azure Service Fabric výkonu monitorování | Microsoft Docs"
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
ms.openlocfilehash: 9d63148c182c705b6b49733c59ed8fdd13872d72
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="performance-metrics"></a><span data-ttu-id="376df-103">Metriky výkonu</span><span class="sxs-lookup"><span data-stu-id="376df-103">Performance metrics</span></span>

<span data-ttu-id="376df-104">Metriky by měl být shromažďovány pochopit výkon vašeho clusteru, jakož i aplikace běžící v ní.</span><span class="sxs-lookup"><span data-stu-id="376df-104">Metrics should be collected to understand the performance of your cluster as well as the applications running in it.</span></span> <span data-ttu-id="376df-105">U clusterů Service Fabric doporučujeme shromažďování následující čítače výkonu.</span><span class="sxs-lookup"><span data-stu-id="376df-105">For Service Fabric clusters, we recommend collecting the following performance counters.</span></span>

## <a name="nodes"></a><span data-ttu-id="376df-106">Uzly</span><span class="sxs-lookup"><span data-stu-id="376df-106">Nodes</span></span>

<span data-ttu-id="376df-107">Pro počítače v clusteru vezměte v úvahu shromažďování následující čítače výkonu pro lepší pochopení zatížení na každém počítači a proveďte příslušné clusteru škálování rozhodnutí.</span><span class="sxs-lookup"><span data-stu-id="376df-107">For the machines in your cluster, consider collecting the following performance counters to better understand the load on each machine and make appropriate cluster scaling decisions.</span></span>

| <span data-ttu-id="376df-108">Kategorie čítače</span><span class="sxs-lookup"><span data-stu-id="376df-108">Counter Category</span></span> | <span data-ttu-id="376df-109">Název čítače</span><span class="sxs-lookup"><span data-stu-id="376df-109">Counter Name</span></span> |
| --- | --- |
| <span data-ttu-id="376df-110">Fyzický disk (na disku)</span><span class="sxs-lookup"><span data-stu-id="376df-110">PhysicalDisk(per Disk)</span></span> | <span data-ttu-id="376df-111">Střední</span><span class="sxs-lookup"><span data-stu-id="376df-111">Avg.</span></span> <span data-ttu-id="376df-112">Délka fronty disku pro čtení</span><span class="sxs-lookup"><span data-stu-id="376df-112">Disk Read Queue Length</span></span> |
| <span data-ttu-id="376df-113">Fyzický disk (na disku)</span><span class="sxs-lookup"><span data-stu-id="376df-113">PhysicalDisk(per Disk)</span></span> | <span data-ttu-id="376df-114">Střední</span><span class="sxs-lookup"><span data-stu-id="376df-114">Avg.</span></span> <span data-ttu-id="376df-115">Délka fronty disku zápisu</span><span class="sxs-lookup"><span data-stu-id="376df-115">Disk Write Queue Length</span></span> |
| <span data-ttu-id="376df-116">Fyzický disk (na disku)</span><span class="sxs-lookup"><span data-stu-id="376df-116">PhysicalDisk(per Disk)</span></span> | <span data-ttu-id="376df-117">Střední</span><span class="sxs-lookup"><span data-stu-id="376df-117">Avg.</span></span> <span data-ttu-id="376df-118">Doba disku/čtení</span><span class="sxs-lookup"><span data-stu-id="376df-118">Disk sec/Read</span></span> |
| <span data-ttu-id="376df-119">Fyzický disk (na disku)</span><span class="sxs-lookup"><span data-stu-id="376df-119">PhysicalDisk(per Disk)</span></span> | <span data-ttu-id="376df-120">Střední</span><span class="sxs-lookup"><span data-stu-id="376df-120">Avg.</span></span> <span data-ttu-id="376df-121">Doba disku/zápis</span><span class="sxs-lookup"><span data-stu-id="376df-121">Disk sec/Write</span></span> |
| <span data-ttu-id="376df-122">Fyzický disk (na disku)</span><span class="sxs-lookup"><span data-stu-id="376df-122">PhysicalDisk(per Disk)</span></span> | <span data-ttu-id="376df-123">Čtení disku/s</span><span class="sxs-lookup"><span data-stu-id="376df-123">Disk Reads/sec</span></span> |
| <span data-ttu-id="376df-124">Fyzický disk (na disku)</span><span class="sxs-lookup"><span data-stu-id="376df-124">PhysicalDisk(per Disk)</span></span> | <span data-ttu-id="376df-125">Čtení z disku bajtů/s</span><span class="sxs-lookup"><span data-stu-id="376df-125">Disk Read Bytes/sec</span></span> |
| <span data-ttu-id="376df-126">Fyzický disk (na disku)</span><span class="sxs-lookup"><span data-stu-id="376df-126">PhysicalDisk(per Disk)</span></span> | <span data-ttu-id="376df-127">Zápis disku/s</span><span class="sxs-lookup"><span data-stu-id="376df-127">Disk Writes/sec</span></span> |
| <span data-ttu-id="376df-128">Fyzický disk (na disku)</span><span class="sxs-lookup"><span data-stu-id="376df-128">PhysicalDisk(per Disk)</span></span> | <span data-ttu-id="376df-129">Bajty zapisování na disk/s</span><span class="sxs-lookup"><span data-stu-id="376df-129">Disk Write Bytes/sec</span></span> |
| <span data-ttu-id="376df-130">Memory (Paměť)</span><span class="sxs-lookup"><span data-stu-id="376df-130">Memory</span></span> | <span data-ttu-id="376df-131">Počet MB k dispozici</span><span class="sxs-lookup"><span data-stu-id="376df-131">Available MBytes</span></span> |
| <span data-ttu-id="376df-132">Soubor stránkování</span><span class="sxs-lookup"><span data-stu-id="376df-132">PagingFile</span></span> | <span data-ttu-id="376df-133">% Využití</span><span class="sxs-lookup"><span data-stu-id="376df-133">% Usage</span></span> |
| <span data-ttu-id="376df-134">Process(Total)</span><span class="sxs-lookup"><span data-stu-id="376df-134">Process(Total)</span></span> | <span data-ttu-id="376df-135">% Času procesoru</span><span class="sxs-lookup"><span data-stu-id="376df-135">% Processor Time</span></span> |
| <span data-ttu-id="376df-136">Proces (pro službu)</span><span class="sxs-lookup"><span data-stu-id="376df-136">Process (per service)</span></span> | <span data-ttu-id="376df-137">% Času procesoru</span><span class="sxs-lookup"><span data-stu-id="376df-137">% Processor Time</span></span> |
| <span data-ttu-id="376df-138">Proces (pro službu)</span><span class="sxs-lookup"><span data-stu-id="376df-138">Process (per service)</span></span> | <span data-ttu-id="376df-139">ID procesu</span><span class="sxs-lookup"><span data-stu-id="376df-139">ID Process</span></span> |
| <span data-ttu-id="376df-140">Proces (pro službu)</span><span class="sxs-lookup"><span data-stu-id="376df-140">Process (per service)</span></span> | <span data-ttu-id="376df-141">Soukromé bajty</span><span class="sxs-lookup"><span data-stu-id="376df-141">Private Bytes</span></span> |
| <span data-ttu-id="376df-142">Proces (pro službu)</span><span class="sxs-lookup"><span data-stu-id="376df-142">Process (per service)</span></span> | <span data-ttu-id="376df-143">Počet vláken</span><span class="sxs-lookup"><span data-stu-id="376df-143">Thread Count</span></span> |
| <span data-ttu-id="376df-144">Proces (pro službu)</span><span class="sxs-lookup"><span data-stu-id="376df-144">Process (per service)</span></span> | <span data-ttu-id="376df-145">Virtuální bajty</span><span class="sxs-lookup"><span data-stu-id="376df-145">Virtual Bytes</span></span> |
| <span data-ttu-id="376df-146">Proces (pro službu)</span><span class="sxs-lookup"><span data-stu-id="376df-146">Process (per service)</span></span> | <span data-ttu-id="376df-147">Pracovní sada</span><span class="sxs-lookup"><span data-stu-id="376df-147">Working Set</span></span> |
| <span data-ttu-id="376df-148">Proces (pro službu)</span><span class="sxs-lookup"><span data-stu-id="376df-148">Process (per service)</span></span> | <span data-ttu-id="376df-149">Pracovní sada – privátní</span><span class="sxs-lookup"><span data-stu-id="376df-149">Working Set - Private</span></span> |

## <a name="net-applications-and-services"></a><span data-ttu-id="376df-150">Aplikace .NET a služby</span><span class="sxs-lookup"><span data-stu-id="376df-150">.NET applications and services</span></span>

<span data-ttu-id="376df-151">Shromážděte následující čítače, pokud nasazujete služeb .NET do clusteru.</span><span class="sxs-lookup"><span data-stu-id="376df-151">Collect the following counters if you are deploying .NET services to your cluster.</span></span> 

| <span data-ttu-id="376df-152">Kategorie čítače</span><span class="sxs-lookup"><span data-stu-id="376df-152">Counter Category</span></span> | <span data-ttu-id="376df-153">Název čítače</span><span class="sxs-lookup"><span data-stu-id="376df-153">Counter Name</span></span> |
| --- | --- |
| <span data-ttu-id="376df-154">Využívání paměti rozhraním .NET CLR (pro službu)</span><span class="sxs-lookup"><span data-stu-id="376df-154">.NET CLR Memory (per service)</span></span> | <span data-ttu-id="376df-155">ID procesu</span><span class="sxs-lookup"><span data-stu-id="376df-155">Process ID</span></span> |
| <span data-ttu-id="376df-156">Využívání paměti rozhraním .NET CLR (pro službu)</span><span class="sxs-lookup"><span data-stu-id="376df-156">.NET CLR Memory (per service)</span></span> | <span data-ttu-id="376df-157"># Celkový počet potvrzených bajtů</span><span class="sxs-lookup"><span data-stu-id="376df-157"># Total committed Bytes</span></span> |
| <span data-ttu-id="376df-158">Využívání paměti rozhraním .NET CLR (pro službu)</span><span class="sxs-lookup"><span data-stu-id="376df-158">.NET CLR Memory (per service)</span></span> | <span data-ttu-id="376df-159"># Celkový počet vyhrazené bajtů</span><span class="sxs-lookup"><span data-stu-id="376df-159"># Total reserved Bytes</span></span> |
| <span data-ttu-id="376df-160">Využívání paměti rozhraním .NET CLR (pro službu)</span><span class="sxs-lookup"><span data-stu-id="376df-160">.NET CLR Memory (per service)</span></span> | <span data-ttu-id="376df-161">Počet bajtů ve všech haldách</span><span class="sxs-lookup"><span data-stu-id="376df-161"># Bytes in all Heaps</span></span> |
| <span data-ttu-id="376df-162">Využívání paměti rozhraním .NET CLR (pro službu)</span><span class="sxs-lookup"><span data-stu-id="376df-162">.NET CLR Memory (per service)</span></span> | <span data-ttu-id="376df-163">Počet úklidů 0</span><span class="sxs-lookup"><span data-stu-id="376df-163"># Gen 0 Collections</span></span> |
| <span data-ttu-id="376df-164">Využívání paměti rozhraním .NET CLR (pro službu)</span><span class="sxs-lookup"><span data-stu-id="376df-164">.NET CLR Memory (per service)</span></span> | <span data-ttu-id="376df-165"># 1. generace kolekce</span><span class="sxs-lookup"><span data-stu-id="376df-165"># Gen 1 Collections</span></span> |
| <span data-ttu-id="376df-166">Využívání paměti rozhraním .NET CLR (pro službu)</span><span class="sxs-lookup"><span data-stu-id="376df-166">.NET CLR Memory (per service)</span></span> | <span data-ttu-id="376df-167"># Úklidů 2. generace</span><span class="sxs-lookup"><span data-stu-id="376df-167"># Gen 2 Collections</span></span> |
| <span data-ttu-id="376df-168">Využívání paměti rozhraním .NET CLR (pro službu)</span><span class="sxs-lookup"><span data-stu-id="376df-168">.NET CLR Memory (per service)</span></span> | <span data-ttu-id="376df-169">Čas</span><span class="sxs-lookup"><span data-stu-id="376df-169">% Time in GC</span></span> |

### <a name="service-fabrics-custom-performance-counters"></a><span data-ttu-id="376df-170">Čítače výkonu vlastní Service Fabric</span><span class="sxs-lookup"><span data-stu-id="376df-170">Service Fabric's custom performance counters</span></span>

<span data-ttu-id="376df-171">Service Fabric generuje vyžadovat značné množství vlastní čítače výkonu.</span><span class="sxs-lookup"><span data-stu-id="376df-171">Service Fabric generates a substantial amount of custom performance counters.</span></span> <span data-ttu-id="376df-172">Pokud máte sadu SDK nainstalovat, v aplikaci sledování výkonu uvidíte kompletní seznam na počítač se systémem Windows (Start > Sledování výkonu).</span><span class="sxs-lookup"><span data-stu-id="376df-172">If you have the SDK installed, you can see the comprehensive list on your Windows machine in your Performance Monitor application (Start > Performance Monitor).</span></span> 

<span data-ttu-id="376df-173">V aplikacích nasazujete do clusteru, pokud používáte Reliable Actors, přidejte countes z `Service Fabric Actor` a `Service Fabric Actor Method` kategorie (viz [Service Fabric spolehlivé aktéři diagnostiky](service-fabric-reliable-actors-diagnostics.md)).</span><span class="sxs-lookup"><span data-stu-id="376df-173">In the applications you are deploying to your cluster, if you are using Reliable Actors, add countes from `Service Fabric Actor` and `Service Fabric Actor Method` categories (see [Service Fabric Reliable Actors Diagnostics](service-fabric-reliable-actors-diagnostics.md)).</span></span>

<span data-ttu-id="376df-174">Pokud používáte spolehlivé služby, podobně jako máme `Service Fabric Service` a `Service Fabric Service Method` kategorie čítače, které mají shromažďovat čítače z.</span><span class="sxs-lookup"><span data-stu-id="376df-174">If you use Reliable Services, we similarly have `Service Fabric Service` and `Service Fabric Service Method` counter categories that you should collect counters from.</span></span> 

<span data-ttu-id="376df-175">Pokud používáte spolehlivé kolekcí, doporučujeme přidání `Avg. Transaction ms/Commit` z `Service Fabric Transactional Replicator` ke shromažďování latenci průměrná potvrzení za metrika transakce.</span><span class="sxs-lookup"><span data-stu-id="376df-175">If you use Reliable Collections, we recommend adding the `Avg. Transaction ms/Commit` from the `Service Fabric Transactional Replicator` to collect the average commit latency per transaction metric.</span></span>


## <a name="next-steps"></a><span data-ttu-id="376df-176">Další kroky</span><span class="sxs-lookup"><span data-stu-id="376df-176">Next steps</span></span>

* <span data-ttu-id="376df-177">Další informace o [generování událostí na úrovni platformy](service-fabric-diagnostics-event-generation-infra.md) v Service Fabric</span><span class="sxs-lookup"><span data-stu-id="376df-177">Learn more about [event generation at the platform level](service-fabric-diagnostics-event-generation-infra.md) in Service Fabric</span></span>
* <span data-ttu-id="376df-178">Shromažďování metrik výkonu prostřednictvím [Azure Diagnostics](service-fabric-diagnostics-event-aggregation-wad.md)</span><span class="sxs-lookup"><span data-stu-id="376df-178">Collect performance metrics through [Azure Diagnostics](service-fabric-diagnostics-event-aggregation-wad.md)</span></span>
