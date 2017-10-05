---
title: "Vyvolat Chaos v prostředí clusterů Service Fabric | Microsoft Docs"
description: "Použití clusteru Analysis Service API a pravděpodobnost vkládání ke správě Chaos v clusteru."
services: service-fabric
documentationcenter: .net
author: motanv
manager: anmola
editor: motanv
ms.assetid: 2bd13443-3478-4382-9a5a-1f6c6b32bfc9
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/09/2017
ms.author: motanv
ms.openlocfilehash: 3b3b93bc9ec5ecdcfc289e5b62e84de6aa4172ed
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="induce-controlled-chaos-in-service-fabric-clusters"></a><span data-ttu-id="0c4fa-103">Vyvolat řízené Chaos v prostředí clusterů Service Fabric</span><span class="sxs-lookup"><span data-stu-id="0c4fa-103">Induce controlled Chaos in Service Fabric clusters</span></span>
<span data-ttu-id="0c4fa-104">Ve velkém měřítku distribuovaných systémů, jako jsou ze své podstaty nespolehlivé cloudových infrastruktur.</span><span class="sxs-lookup"><span data-stu-id="0c4fa-104">Large-scale distributed systems like cloud infrastructures are inherently unreliable.</span></span> <span data-ttu-id="0c4fa-105">Azure Service Fabric umožňuje vývojářům psát spolehlivé distribuované služby nespolehlivé infrastruktuře.</span><span class="sxs-lookup"><span data-stu-id="0c4fa-105">Azure Service Fabric enables developers to write reliable distributed services on top of an unreliable infrastructure.</span></span> <span data-ttu-id="0c4fa-106">Zápis robustní distribuované služby nespolehlivé infrastruktuře, vývojáři potřeba otestovat stability svých služeb, při odpovídající nespolehlivé infrastruktury prochází přes přechodů mezi stavy složité z důvodu chyb.</span><span class="sxs-lookup"><span data-stu-id="0c4fa-106">To write robust distributed services on top of an unreliable infrastructure, developers need to be able to test the stability of their services while the underlying unreliable infrastructure is going through complicated state transitions due to faults.</span></span>

<span data-ttu-id="0c4fa-107">[Vkládání selhání a clusteru Analysis Service](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-testability-overview) (také označované jako služba analýza selhání) poskytuje vývojářům možnost způsobit chyby k otestování svých služeb.</span><span class="sxs-lookup"><span data-stu-id="0c4fa-107">The [Fault Injection and Cluster Analysis Service](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-testability-overview) (also known as the Fault Analysis Service) gives developers the ability to induce faults to test their services.</span></span> <span data-ttu-id="0c4fa-108">Tyto cílové simulated chyb, jako je třeba [restartování oddíl](https://docs.microsoft.com/en-us/powershell/module/servicefabric/start-servicefabricpartitionrestart?view=azureservicefabricps), může pomoci prověření nejběžnější přechodů mezi stavy.</span><span class="sxs-lookup"><span data-stu-id="0c4fa-108">These targeted simulated faults, like [restarting a partition](https://docs.microsoft.com/en-us/powershell/module/servicefabric/start-servicefabricpartitionrestart?view=azureservicefabricps), can help exercise the most common state transitions.</span></span> <span data-ttu-id="0c4fa-109">Ale cílové simulované chyb jsou tendenční podle definice a proto může dojít chyby které zobrazují se pouze v pevném předpovědi, dlouhé a komplikované posloupnost přechodů mezi stavy.</span><span class="sxs-lookup"><span data-stu-id="0c4fa-109">However targeted simulated faults are biased by definition and thus may miss bugs that show up only in hard-to-predict, long and complicated sequence of state transitions.</span></span> <span data-ttu-id="0c4fa-110">Pro neposunutého testování, můžete použít Chaos.</span><span class="sxs-lookup"><span data-stu-id="0c4fa-110">For an unbiased testing, you can use Chaos.</span></span>

<span data-ttu-id="0c4fa-111">Chaos simuluje pravidelné, prokládaná chyb (řádně i vynuceném) v rámci clusteru přes dlouhou dobu.</span><span class="sxs-lookup"><span data-stu-id="0c4fa-111">Chaos simulates periodic, interleaved faults (both graceful and ungraceful) throughout the cluster over extended periods of time.</span></span> <span data-ttu-id="0c4fa-112">Po konfiguraci Chaos s rychlost a druh chyb, můžete začít Chaos prostřednictvím jazyka C# nebo rozhraní API prostředí Powershell spustit generování chyb v clusteru a vaše služby.</span><span class="sxs-lookup"><span data-stu-id="0c4fa-112">Once you have configured Chaos with the rate and the kind of faults, you can start Chaos through C# or Powershell API to start generating faults in the cluster and in your services.</span></span> <span data-ttu-id="0c4fa-113">Chaos spuštění pro zadané časové období (například pro jednu hodinu), po které zastaví Chaos lze nastavit automaticky nebo můžete volat rozhraní API StopChaos (C# nebo prostředí Powershell) pro zastavení kdykoli.</span><span class="sxs-lookup"><span data-stu-id="0c4fa-113">You can configure Chaos to run for a specified time period (for example, for one hour), after which Chaos stops automatically, or you can call StopChaos API (C# or Powershell) to stop it at any time.</span></span>

> [!NOTE]
> <span data-ttu-id="0c4fa-114">V současné podobě Chaos indukuje pouze bezpečné chyb, což znamená, že chybí externí chyb ztrátě kvora nebo ztrátě dat se nikdy neprovádí.</span><span class="sxs-lookup"><span data-stu-id="0c4fa-114">In its current form, Chaos induces only safe faults, which implies that in the absence of external faults a quorum loss, or data loss never occurs.</span></span>
>

<span data-ttu-id="0c4fa-115">Je spuštěn Chaos, vyvolá různé události, které zaznamenat stav spuštění v tuto chvíli.</span><span class="sxs-lookup"><span data-stu-id="0c4fa-115">While Chaos is running, it produces different events that capture the state of the run at the moment.</span></span> <span data-ttu-id="0c4fa-116">Například ExecutingFaultsEvent obsahuje všechny chyb, které se rozhodla Chaos provést v této iteraci.</span><span class="sxs-lookup"><span data-stu-id="0c4fa-116">For example, an ExecutingFaultsEvent contains all the faults that Chaos has decided to execute in that iteration.</span></span> <span data-ttu-id="0c4fa-117">ValidationFailedEvent obsahuje podrobnosti o selhání ověření (stavu nebo stabilitu problémy), který byl nalezen během ověření clusteru.</span><span class="sxs-lookup"><span data-stu-id="0c4fa-117">A ValidationFailedEvent contains the details of a validation failure (health or stability issues) that was found during the validation of the cluster.</span></span> <span data-ttu-id="0c4fa-118">Rozhraní API GetChaosReport (C# nebo prostředí Powershell) Chcete-li získat sestavu Chaos spustí můžete vyvolat.</span><span class="sxs-lookup"><span data-stu-id="0c4fa-118">You can invoke the GetChaosReport API (C# or Powershell) to get the report of Chaos runs.</span></span> <span data-ttu-id="0c4fa-119">Tyto události získat uchovávané v [spolehlivé slovník](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-reliable-services-reliable-collections), který má zásady zkrácení závisí na dvě konfigurace: **MaxStoredChaosEventCount** (výchozí hodnota je 25 000) a **StoredActionCleanupIntervalInSeconds** (výchozí hodnota je 3600).</span><span class="sxs-lookup"><span data-stu-id="0c4fa-119">These events get persisted in a [reliable dictionary](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-reliable-services-reliable-collections), which has a truncation policy dictated by two configurations: **MaxStoredChaosEventCount** (default value is 25000) and **StoredActionCleanupIntervalInSeconds** (default value is 3600).</span></span> <span data-ttu-id="0c4fa-120">Každý *StoredActionCleanupIntervalInSeconds* Chaos kontroly a všechny, ale nejnovější *MaxStoredChaosEventCount* události, jsou vymazány ze slovníku spolehlivé.</span><span class="sxs-lookup"><span data-stu-id="0c4fa-120">Every *StoredActionCleanupIntervalInSeconds* Chaos checks and all but the most recent *MaxStoredChaosEventCount* events, are purged from the reliable dictionary.</span></span>

## <a name="faults-induced-in-chaos"></a><span data-ttu-id="0c4fa-121">Vyvolané v Chaos chyb</span><span class="sxs-lookup"><span data-stu-id="0c4fa-121">Faults induced in Chaos</span></span>
<span data-ttu-id="0c4fa-122">Chaos generuje chyby napříč celý cluster Service Fabric a komprimaci chyb, které jsou zobrazená v měsíců nebo let do několik hodin.</span><span class="sxs-lookup"><span data-stu-id="0c4fa-122">Chaos generates faults across the entire Service Fabric cluster and compresses faults that are seen in months or years into a few hours.</span></span> <span data-ttu-id="0c4fa-123">Kombinace prokládaná chyb s vysokou odolnost rychlost vyhledá náročnějších případech, které jinak může být načteni.</span><span class="sxs-lookup"><span data-stu-id="0c4fa-123">The combination of interleaved faults with the high fault rate finds corner cases that may otherwise be missed.</span></span> <span data-ttu-id="0c4fa-124">Toto cvičení Chaos vede k významné zlepšení kvality kódu služby.</span><span class="sxs-lookup"><span data-stu-id="0c4fa-124">This exercise of Chaos leads to a significant improvement in the code quality of the service.</span></span>

<span data-ttu-id="0c4fa-125">Chaos indukuje chyb z následujících kategorií:</span><span class="sxs-lookup"><span data-stu-id="0c4fa-125">Chaos induces faults from the following categories:</span></span>

* <span data-ttu-id="0c4fa-126">Restartovat uzel</span><span class="sxs-lookup"><span data-stu-id="0c4fa-126">Restart a node</span></span>
* <span data-ttu-id="0c4fa-127">Restartujte nasazený balíček kódu</span><span class="sxs-lookup"><span data-stu-id="0c4fa-127">Restart a deployed code package</span></span>
* <span data-ttu-id="0c4fa-128">Odstranění repliky</span><span class="sxs-lookup"><span data-stu-id="0c4fa-128">Remove a replica</span></span>
* <span data-ttu-id="0c4fa-129">Restartujte repliky</span><span class="sxs-lookup"><span data-stu-id="0c4fa-129">Restart a replica</span></span>
* <span data-ttu-id="0c4fa-130">Přesunutí primární repliky (Konfigurovat)</span><span class="sxs-lookup"><span data-stu-id="0c4fa-130">Move a primary replica (configurable)</span></span>
* <span data-ttu-id="0c4fa-131">Přesunutí sekundární repliky (Konfigurovat)</span><span class="sxs-lookup"><span data-stu-id="0c4fa-131">Move a secondary replica (configurable)</span></span>

<span data-ttu-id="0c4fa-132">Chaos běží ve více iterací.</span><span class="sxs-lookup"><span data-stu-id="0c4fa-132">Chaos runs in multiple iterations.</span></span> <span data-ttu-id="0c4fa-133">Každé iteraci se skládá z chyb a ověření clusteru pro toto období.</span><span class="sxs-lookup"><span data-stu-id="0c4fa-133">Each iteration consists of faults and cluster validation for the specified period.</span></span> <span data-ttu-id="0c4fa-134">Můžete nakonfigurovat čas strávený clusteru stabilizovat a pro ověření úspěšné.</span><span class="sxs-lookup"><span data-stu-id="0c4fa-134">You can configure the time spent for the cluster to stabilize and for validation to succeed.</span></span> <span data-ttu-id="0c4fa-135">Pokud selhání naleznete v ověření clusteru, Chaos generuje a přetrvává ValidationFailedEvent s časovým razítkem UTC a podrobnosti o chybě.</span><span class="sxs-lookup"><span data-stu-id="0c4fa-135">If a failure is found in cluster validation, Chaos generates and persists a ValidationFailedEvent with the UTC timestamp and the failure details.</span></span> <span data-ttu-id="0c4fa-136">Představte si třeba instanci Chaos, který je nastaven na spouštění jednu hodinu s maximálně tři souběžných chyb.</span><span class="sxs-lookup"><span data-stu-id="0c4fa-136">For example, consider an instance of Chaos that is set to run for an hour with a maximum of three concurrent faults.</span></span> <span data-ttu-id="0c4fa-137">Chaos indukuje tři chyb a poté ověří stav clusteru.</span><span class="sxs-lookup"><span data-stu-id="0c4fa-137">Chaos induces three faults, and then validates the cluster health.</span></span> <span data-ttu-id="0c4fa-138">Ho iteruje předchozí krok, dokud není explicitně zastaven prostřednictvím rozhraní API StopChaosAsync nebo hodinových předá.</span><span class="sxs-lookup"><span data-stu-id="0c4fa-138">It iterates through the previous step until it is explicitly stopped through the StopChaosAsync API or one-hour passes.</span></span> <span data-ttu-id="0c4fa-139">Pokud cluster se změní na není v pořádku v kteroukoli iteraci (to znamená, že ho není stabilizovat v rámci MaxClusterStabilizationTimeout předané), Chaos generuje ValidationFailedEvent.</span><span class="sxs-lookup"><span data-stu-id="0c4fa-139">If the cluster becomes unhealthy in any iteration (that is, it does not stabilize within the passed-in MaxClusterStabilizationTimeout), Chaos generates a ValidationFailedEvent.</span></span> <span data-ttu-id="0c4fa-140">Tato událost označuje, že něco nepovede a může být třeba další šetření.</span><span class="sxs-lookup"><span data-stu-id="0c4fa-140">This event indicates that something has gone wrong and might need further investigation.</span></span>

<span data-ttu-id="0c4fa-141">Získat chyb, které Chaos vyvolané, můžete použít rozhraní API GetChaosReport (prostředí powershell nebo C#).</span><span class="sxs-lookup"><span data-stu-id="0c4fa-141">To get which faults Chaos induced, you can use GetChaosReport API (powershell or C#).</span></span> <span data-ttu-id="0c4fa-142">Rozhraní API získá další segment Chaos sestavy na základě token pro pokračování předané nebo předané čas rozsahu.</span><span class="sxs-lookup"><span data-stu-id="0c4fa-142">The API gets the next segment of the Chaos report based on the passed-in continuation token or the passed-in time-range.</span></span> <span data-ttu-id="0c4fa-143">Můžete buď zadat ContinuationToken získat další segment Chaos sestavy nebo můžete zadat časový rozsah prostřednictvím StartTimeUtc a EndTimeUtc, ale ContinuationToken a časový rozsah nelze zadat ve stejném volání.</span><span class="sxs-lookup"><span data-stu-id="0c4fa-143">You can either specify the ContinuationToken to get the next segment of the Chaos report or you can specify the time-range through StartTimeUtc and EndTimeUtc, but you cannot specify both the ContinuationToken and the time-range in the same call.</span></span> <span data-ttu-id="0c4fa-144">Pokud nejsou k dispozici více než 100 Chaos události, Chaos sestavy je vrácený v segmenty, kde segment obsahuje více než 100 Chaos událostí.</span><span class="sxs-lookup"><span data-stu-id="0c4fa-144">When there are more than 100 Chaos events, the Chaos report is returned in segments where a segment contains no more than 100 Chaos events.</span></span>

## <a name="important-configuration-options"></a><span data-ttu-id="0c4fa-145">Důležité konfigurační možnosti</span><span class="sxs-lookup"><span data-stu-id="0c4fa-145">Important configuration options</span></span>
* <span data-ttu-id="0c4fa-146">**TimeToRun**: celkový čas která Chaos spouští před dokončením s úspěch.</span><span class="sxs-lookup"><span data-stu-id="0c4fa-146">**TimeToRun**: Total time that Chaos runs before it finishes with success.</span></span> <span data-ttu-id="0c4fa-147">Předtím, než byl spuštěn po dobu TimeToRun prostřednictvím rozhraní API StopChaos můžete zastavit Chaos.</span><span class="sxs-lookup"><span data-stu-id="0c4fa-147">You can stop Chaos before it has run for the TimeToRun period through the StopChaos API.</span></span>

* <span data-ttu-id="0c4fa-148">**MaxClusterStabilizationTimeout**: maximální množství času čekání na clusteru se nezotavila před generovala ValidationFailedEvent.</span><span class="sxs-lookup"><span data-stu-id="0c4fa-148">**MaxClusterStabilizationTimeout**: The maximum amount of time to wait for the cluster to become healthy before producing a ValidationFailedEvent.</span></span> <span data-ttu-id="0c4fa-149">Toto čekání je ke snížení zatížení v clusteru, zatímco probíhá obnovení.</span><span class="sxs-lookup"><span data-stu-id="0c4fa-149">This wait is to reduce the load on the cluster while it is recovering.</span></span> <span data-ttu-id="0c4fa-150">Provést kontroly jsou:</span><span class="sxs-lookup"><span data-stu-id="0c4fa-150">The checks performed are:</span></span>
  * <span data-ttu-id="0c4fa-151">Pokud stav clusteru je v pořádku</span><span class="sxs-lookup"><span data-stu-id="0c4fa-151">If the cluster health is OK</span></span>
  * <span data-ttu-id="0c4fa-152">Pokud stav služby je v pořádku.</span><span class="sxs-lookup"><span data-stu-id="0c4fa-152">If the service health is OK</span></span>
  * <span data-ttu-id="0c4fa-153">Pokud cílový repliky nastavit velikost se dá dosáhnout oddílu služby</span><span class="sxs-lookup"><span data-stu-id="0c4fa-153">If the target replica set size is achieved for the service partition</span></span>
  * <span data-ttu-id="0c4fa-154">Že neexistuje žádná replik InBuild</span><span class="sxs-lookup"><span data-stu-id="0c4fa-154">That no InBuild replicas exist</span></span>
* <span data-ttu-id="0c4fa-155">**MaxConcurrentFaults**: maximální počet souběžných chyb, které jsou v každé iteraci vyvolané.</span><span class="sxs-lookup"><span data-stu-id="0c4fa-155">**MaxConcurrentFaults**: The maximum number of concurrent faults that are induced in each iteration.</span></span> <span data-ttu-id="0c4fa-156">Je vyšší číslo, tím agresivnější Chaos a převzetí služeb při selhání a kombinace přechod stavu, které procházejí clusteru jsou i složitější.</span><span class="sxs-lookup"><span data-stu-id="0c4fa-156">The higher the number, the more aggressive Chaos is and the failovers and the state transition combinations that the cluster goes through are also more complex.</span></span> 

> [!NOTE]
> <span data-ttu-id="0c4fa-157">Bez ohledu na to jak velkou hodnotu *MaxConcurrentFaults* má Chaos zaručuje – chybí externí chyb - neexistuje ztrátě kvora nebo ztrátě dat.</span><span class="sxs-lookup"><span data-stu-id="0c4fa-157">Regardless how high a value *MaxConcurrentFaults* has, Chaos guarantees - in the absence of external faults - there is no quorum loss or data loss.</span></span>
>

* <span data-ttu-id="0c4fa-158">**EnableMoveReplicaFaults**: Povolí nebo zakáže chyb, které způsobují primární nebo sekundární repliky přesunout.</span><span class="sxs-lookup"><span data-stu-id="0c4fa-158">**EnableMoveReplicaFaults**: Enables or disables the faults that cause the primary or secondary replicas to move.</span></span> <span data-ttu-id="0c4fa-159">Tyto chyby jsou ve výchozím nastavení zakázány.</span><span class="sxs-lookup"><span data-stu-id="0c4fa-159">These faults are disabled by default.</span></span>
* <span data-ttu-id="0c4fa-160">**WaitTimeBetweenIterations**: dobu čekání mezi iterací.</span><span class="sxs-lookup"><span data-stu-id="0c4fa-160">**WaitTimeBetweenIterations**: The amount of time to wait between iterations.</span></span> <span data-ttu-id="0c4fa-161">To znamená množství času Chaos se pozastaví po provedení zaokrouhlit chyb a nutnosti dokončení odpovídající ověření stavu clusteru.</span><span class="sxs-lookup"><span data-stu-id="0c4fa-161">That is, the amount of time Chaos will pause after having executed a round of faults and having finished the corresponding validation of the health of the cluster.</span></span> <span data-ttu-id="0c4fa-162">Čím vyšší hodnota, čím nižší je míra vkládání průměrná selhání.</span><span class="sxs-lookup"><span data-stu-id="0c4fa-162">The higher the value, the lower is the average fault injection rate.</span></span>
* <span data-ttu-id="0c4fa-163">**WaitTimeBetweenFaults**: dobu čekání mezi dvě po sobě jdoucích chyb v jednom iterací.</span><span class="sxs-lookup"><span data-stu-id="0c4fa-163">**WaitTimeBetweenFaults**: The amount of time to wait between two consecutive faults in a single iteration.</span></span> <span data-ttu-id="0c4fa-164">Vyšší hodnota, čím nižší současnému (nebo překryv mezi) chyb.</span><span class="sxs-lookup"><span data-stu-id="0c4fa-164">The higher the value, the lower the concurrency of (or the overlap between) faults.</span></span>
* <span data-ttu-id="0c4fa-165">**ClusterHealthPolicy**: zásady stavu clusteru se používá k ověření stavu clusteru mezi Chaos iterací.</span><span class="sxs-lookup"><span data-stu-id="0c4fa-165">**ClusterHealthPolicy**: Cluster health policy is used to validate the health of the cluster in between Chaos iterations.</span></span> <span data-ttu-id="0c4fa-166">Pokud stav clusteru došlo k chybě nebo pokud se stane neočekávané výjimce během zpracování chyby, budou Chaos Počkejte 30 minut před další-kontrolou stavu - poskytnout chvíli recuperate clusteru.</span><span class="sxs-lookup"><span data-stu-id="0c4fa-166">If the cluster health is in error or if an unexpected exception happens during fault execution, Chaos will wait for 30 minutes before the next health-check - to provide the cluster with some time to recuperate.</span></span>
* <span data-ttu-id="0c4fa-167">**Kontext**: kolekce (řetězec, řetězec) zadejte páry klíč hodnota.</span><span class="sxs-lookup"><span data-stu-id="0c4fa-167">**Context**: A collection of (string, string) type key-value pairs.</span></span> <span data-ttu-id="0c4fa-168">Mapy slouží k zaznamenání informací o Chaos spustit.</span><span class="sxs-lookup"><span data-stu-id="0c4fa-168">The map can be used to record information about the Chaos run.</span></span> <span data-ttu-id="0c4fa-169">Nemůže být více než 100 tyto dvojice a každý řetězec (klíč nebo hodnota) může mít maximálně povolených 4095 znaků dlouhé.</span><span class="sxs-lookup"><span data-stu-id="0c4fa-169">There cannot be more than 100 such pairs and each string (key or value) can be at most 4095 characters long.</span></span> <span data-ttu-id="0c4fa-170">Tato mapa je nastavena podle starter zmatku spustit, aby se volitelně ukládat kontext o konkrétní spustit.</span><span class="sxs-lookup"><span data-stu-id="0c4fa-170">This map is set by the starter of the Chaos run to optionally store the context about the specific run.</span></span>

## <a name="how-to-run-chaos"></a><span data-ttu-id="0c4fa-171">Jak spustit Chaos</span><span class="sxs-lookup"><span data-stu-id="0c4fa-171">How to run Chaos</span></span>

```csharp
using System;
using System.Collections.Generic;
using System.Threading.Tasks;
using System.Fabric;

using System.Diagnostics;
using System.Fabric.Chaos.DataStructures;

class Program
{
    private class ChaosEventComparer : IEqualityComparer<ChaosEvent>
    {
        public bool Equals(ChaosEvent x, ChaosEvent y)
        {
            return x.TimeStampUtc.Equals(y.TimeStampUtc);
        }

        public int GetHashCode(ChaosEvent obj)
        {
            return obj.TimeStampUtc.GetHashCode();
        }
    }

    static void Main(string[] args)
    {
        var clusterConnectionString = "localhost:19000";
        using (var client = new FabricClient(clusterConnectionString))
        {
            var startTimeUtc = DateTime.UtcNow;
            var stabilizationTimeout = TimeSpan.FromSeconds(30.0);
            var timeToRun = TimeSpan.FromMinutes(60.0);
            var maxConcurrentFaults = 3;

            var parameters = new ChaosParameters(
                stabilizationTimeout,
                maxConcurrentFaults,
                true, /* EnableMoveReplicaFault */
                timeToRun);

            try
            {
                client.TestManager.StartChaosAsync(parameters).GetAwaiter().GetResult();
            }
            catch (FabricChaosAlreadyRunningException)
            {
                Console.WriteLine("An instance of Chaos is already running in the cluster.");
            }

            var filter = new ChaosReportFilter(startTimeUtc, DateTime.MaxValue);

            var eventSet = new HashSet<ChaosEvent>(new ChaosEventComparer());

            while (true)
            {
                var report = client.TestManager.GetChaosReportAsync(filter).GetAwaiter().GetResult();

                foreach (var chaosEvent in report.History)
                {
                    if (eventSet.Add(chaosEvent))
                    {
                        Console.WriteLine(chaosEvent);
                    }
                }

                // When Chaos stops, a StoppedEvent is created.
                // If a StoppedEvent is found, exit the loop.
                var lastEvent = report.History.LastOrDefault();

                if (lastEvent is StoppedEvent)
                {
                    break;
                }

                Task.Delay(TimeSpan.FromSeconds(1.0)).GetAwaiter().GetResult();
            }
        }
    }
}
```

```powershell
$connection = "localhost:19000"
$timeToRun = 60
$maxStabilizationTimeSecs = 180
$concurrentFaults = 3
$waitTimeBetweenIterationsSec = 60

Connect-ServiceFabricCluster $connection

$events = @{}
$now = [System.DateTime]::UtcNow

Start-ServiceFabricChaos -TimeToRunMinute $timeToRun -MaxConcurrentFaults $concurrentFaults -MaxClusterStabilizationTimeoutSec $maxStabilizationTimeSecs -EnableMoveReplicaFaults -WaitTimeBetweenIterationsSec $waitTimeBetweenIterationsSec

while($true)
{
    $stopped = $false
    $report = Get-ServiceFabricChaosReport -StartTimeUtc $now -EndTimeUtc ([System.DateTime]::MaxValue)

    foreach ($e in $report.History) {

        if(-Not ($events.Contains($e.TimeStampUtc.Ticks)))
        {
            $events.Add($e.TimeStampUtc.Ticks, $e)
            if($e -is [System.Fabric.Chaos.DataStructures.ValidationFailedEvent])
            {
                Write-Host -BackgroundColor White -ForegroundColor Red $e
            }
            else
            {
                if($e -is [System.Fabric.Chaos.DataStructures.StoppedEvent])
                {
                    $stopped = $true
                }

                Write-Host $e
            }
        }
    }

    if($stopped -eq $true)
    {
        break
    }

    Start-Sleep -Seconds 1
}

Stop-ServiceFabricChaos
```
