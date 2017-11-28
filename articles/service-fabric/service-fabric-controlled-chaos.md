---
title: clustery Chaos v Service Fabric aaaInduce | Microsoft Docs
description: "Použití clusteru Analysis Service API a pravděpodobnost vkládání toomanage Chaos v clusteru hello."
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
ms.openlocfilehash: 7e87cae22645fc4ba52e258471d8f3a4ffdb1cce
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="induce-controlled-chaos-in-service-fabric-clusters"></a><span data-ttu-id="e1f92-103">Vyvolat řízené Chaos v prostředí clusterů Service Fabric</span><span class="sxs-lookup"><span data-stu-id="e1f92-103">Induce controlled Chaos in Service Fabric clusters</span></span>
<span data-ttu-id="e1f92-104">Ve velkém měřítku distribuovaných systémů, jako jsou ze své podstaty nespolehlivé cloudových infrastruktur.</span><span class="sxs-lookup"><span data-stu-id="e1f92-104">Large-scale distributed systems like cloud infrastructures are inherently unreliable.</span></span> <span data-ttu-id="e1f92-105">Azure Service Fabric umožňuje vývojářům toowrite spolehlivé distribuované služby nespolehlivé infrastruktuře.</span><span class="sxs-lookup"><span data-stu-id="e1f92-105">Azure Service Fabric enables developers toowrite reliable distributed services on top of an unreliable infrastructure.</span></span> <span data-ttu-id="e1f92-106">toowrite robustní distribuované služby nespolehlivé infrastruktuře, vývojáři třeba toobe možné tootest hello stabilitu své služby při hello základní nespolehlivé infrastruktury prochází přes přechodů mezi stavy složité kvůli toofaults.</span><span class="sxs-lookup"><span data-stu-id="e1f92-106">toowrite robust distributed services on top of an unreliable infrastructure, developers need toobe able tootest hello stability of their services while hello underlying unreliable infrastructure is going through complicated state transitions due toofaults.</span></span>

<span data-ttu-id="e1f92-107">Hello [vkládání selhání a clusteru Analysis Service](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-testability-overview) (také označované jako hello selhání Analysis Service) poskytuje vývojářům možnost hello tooinduce chyb tootest své služby.</span><span class="sxs-lookup"><span data-stu-id="e1f92-107">hello [Fault Injection and Cluster Analysis Service](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-testability-overview) (also known as hello Fault Analysis Service) gives developers hello ability tooinduce faults tootest their services.</span></span> <span data-ttu-id="e1f92-108">Tyto cílové simulated chyb, jako je třeba [restartování oddíl](https://docs.microsoft.com/en-us/powershell/module/servicefabric/start-servicefabricpartitionrestart?view=azureservicefabricps), může pomoci prověření hello nejběžnější přechodů mezi stavy.</span><span class="sxs-lookup"><span data-stu-id="e1f92-108">These targeted simulated faults, like [restarting a partition](https://docs.microsoft.com/en-us/powershell/module/servicefabric/start-servicefabricpartitionrestart?view=azureservicefabricps), can help exercise hello most common state transitions.</span></span> <span data-ttu-id="e1f92-109">Ale cílové simulované chyb jsou tendenční podle definice a proto může dojít chyby které zobrazují se pouze v pevném předpovědi, dlouhé a komplikované posloupnost přechodů mezi stavy.</span><span class="sxs-lookup"><span data-stu-id="e1f92-109">However targeted simulated faults are biased by definition and thus may miss bugs that show up only in hard-to-predict, long and complicated sequence of state transitions.</span></span> <span data-ttu-id="e1f92-110">Pro neposunutého testování, můžete použít Chaos.</span><span class="sxs-lookup"><span data-stu-id="e1f92-110">For an unbiased testing, you can use Chaos.</span></span>

<span data-ttu-id="e1f92-111">Chaos simuluje pravidelné, prokládaná chyb (řádně i vynuceném) v rámci clusteru hello přes dlouhou dobu.</span><span class="sxs-lookup"><span data-stu-id="e1f92-111">Chaos simulates periodic, interleaved faults (both graceful and ungraceful) throughout hello cluster over extended periods of time.</span></span> <span data-ttu-id="e1f92-112">Po konfiguraci Chaos s hello rychlost a hello druh chyb, můžete začít Chaos prostřednictvím jazyka C# nebo rozhraní API prostředí Powershell toostart generování chyb v clusteru hello a vaše služby.</span><span class="sxs-lookup"><span data-stu-id="e1f92-112">Once you have configured Chaos with hello rate and hello kind of faults, you can start Chaos through C# or Powershell API toostart generating faults in hello cluster and in your services.</span></span> <span data-ttu-id="e1f92-113">Můžete nakonfigurovat Chaos toorun pro zadané časové období (například pro jednu hodinu), po jejímž uplynutí Chaos zastaví automaticky, nebo můžete volat rozhraní API StopChaos (C# nebo prostředí Powershell) toostop ho kdykoli.</span><span class="sxs-lookup"><span data-stu-id="e1f92-113">You can configure Chaos toorun for a specified time period (for example, for one hour), after which Chaos stops automatically, or you can call StopChaos API (C# or Powershell) toostop it at any time.</span></span>

> [!NOTE]
> <span data-ttu-id="e1f92-114">V současné podobě Chaos indukuje pouze bezpečné chyb, což znamená, že hello neexistence externí chyb ztrátě kvora nebo ztrátě dat se nikdy neprovádí.</span><span class="sxs-lookup"><span data-stu-id="e1f92-114">In its current form, Chaos induces only safe faults, which implies that in hello absence of external faults a quorum loss, or data loss never occurs.</span></span>
>

<span data-ttu-id="e1f92-115">Když Chaos je spuštěný, vytváří různé události, které zaznamenat stav hello hello spustit momentálně hello.</span><span class="sxs-lookup"><span data-stu-id="e1f92-115">While Chaos is running, it produces different events that capture hello state of hello run at hello moment.</span></span> <span data-ttu-id="e1f92-116">Například ExecutingFaultsEvent obsahuje všechny hello chyb, Chaos se rozhodla tooexecute v této iteraci.</span><span class="sxs-lookup"><span data-stu-id="e1f92-116">For example, an ExecutingFaultsEvent contains all hello faults that Chaos has decided tooexecute in that iteration.</span></span> <span data-ttu-id="e1f92-117">ValidationFailedEvent obsahuje hello podrobné informace o selhání ověření (stavu nebo stabilitu problémy), který byl nalezen během ověřování hello hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="e1f92-117">A ValidationFailedEvent contains hello details of a validation failure (health or stability issues) that was found during hello validation of hello cluster.</span></span> <span data-ttu-id="e1f92-118">Můžete vyvolat hello GetChaosReport API (C# nebo prostředí Powershell) tooget hello sestavu Chaos spustí.</span><span class="sxs-lookup"><span data-stu-id="e1f92-118">You can invoke hello GetChaosReport API (C# or Powershell) tooget hello report of Chaos runs.</span></span> <span data-ttu-id="e1f92-119">Tyto události získat uchovávané v [spolehlivé slovník](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-reliable-services-reliable-collections), který má zásady zkrácení závisí na dvě konfigurace: **MaxStoredChaosEventCount** (výchozí hodnota je 25 000) a **StoredActionCleanupIntervalInSeconds** (výchozí hodnota je 3600).</span><span class="sxs-lookup"><span data-stu-id="e1f92-119">These events get persisted in a [reliable dictionary](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-reliable-services-reliable-collections), which has a truncation policy dictated by two configurations: **MaxStoredChaosEventCount** (default value is 25000) and **StoredActionCleanupIntervalInSeconds** (default value is 3600).</span></span> <span data-ttu-id="e1f92-120">Každý *StoredActionCleanupIntervalInSeconds* Chaos kontroly a všechny ale hello nejnovější *MaxStoredChaosEventCount* události, jsou vymazány ze slovníku spolehlivé hello.</span><span class="sxs-lookup"><span data-stu-id="e1f92-120">Every *StoredActionCleanupIntervalInSeconds* Chaos checks and all but hello most recent *MaxStoredChaosEventCount* events, are purged from hello reliable dictionary.</span></span>

## <a name="faults-induced-in-chaos"></a><span data-ttu-id="e1f92-121">Vyvolané v Chaos chyb</span><span class="sxs-lookup"><span data-stu-id="e1f92-121">Faults induced in Chaos</span></span>
<span data-ttu-id="e1f92-122">Chaos generuje chyby napříč hello celý cluster Service Fabric a komprimaci chyb, které jsou zobrazená v měsíců nebo let do několik hodin.</span><span class="sxs-lookup"><span data-stu-id="e1f92-122">Chaos generates faults across hello entire Service Fabric cluster and compresses faults that are seen in months or years into a few hours.</span></span> <span data-ttu-id="e1f92-123">kombinace Hello prokládaná chyb hello vysokou odolnost míru vyhledá náročnějších případech, které jinak může být načteni.</span><span class="sxs-lookup"><span data-stu-id="e1f92-123">hello combination of interleaved faults with hello high fault rate finds corner cases that may otherwise be missed.</span></span> <span data-ttu-id="e1f92-124">Toto cvičení Chaos vede tooa výrazné zlepšení kvality kódu hello hello služby.</span><span class="sxs-lookup"><span data-stu-id="e1f92-124">This exercise of Chaos leads tooa significant improvement in hello code quality of hello service.</span></span>

<span data-ttu-id="e1f92-125">Chaos indukuje chyb z hello následujících kategorií:</span><span class="sxs-lookup"><span data-stu-id="e1f92-125">Chaos induces faults from hello following categories:</span></span>

* <span data-ttu-id="e1f92-126">Restartovat uzel</span><span class="sxs-lookup"><span data-stu-id="e1f92-126">Restart a node</span></span>
* <span data-ttu-id="e1f92-127">Restartujte nasazený balíček kódu</span><span class="sxs-lookup"><span data-stu-id="e1f92-127">Restart a deployed code package</span></span>
* <span data-ttu-id="e1f92-128">Odstranění repliky</span><span class="sxs-lookup"><span data-stu-id="e1f92-128">Remove a replica</span></span>
* <span data-ttu-id="e1f92-129">Restartujte repliky</span><span class="sxs-lookup"><span data-stu-id="e1f92-129">Restart a replica</span></span>
* <span data-ttu-id="e1f92-130">Přesunutí primární repliky (Konfigurovat)</span><span class="sxs-lookup"><span data-stu-id="e1f92-130">Move a primary replica (configurable)</span></span>
* <span data-ttu-id="e1f92-131">Přesunutí sekundární repliky (Konfigurovat)</span><span class="sxs-lookup"><span data-stu-id="e1f92-131">Move a secondary replica (configurable)</span></span>

<span data-ttu-id="e1f92-132">Chaos běží ve více iterací.</span><span class="sxs-lookup"><span data-stu-id="e1f92-132">Chaos runs in multiple iterations.</span></span> <span data-ttu-id="e1f92-133">Každé iteraci se skládá z chyb a ověření clusteru pro hello zadané období.</span><span class="sxs-lookup"><span data-stu-id="e1f92-133">Each iteration consists of faults and cluster validation for hello specified period.</span></span> <span data-ttu-id="e1f92-134">Můžete nakonfigurovat hello času stráveného pro hello clusteru toostabilize a toosucceed ověření.</span><span class="sxs-lookup"><span data-stu-id="e1f92-134">You can configure hello time spent for hello cluster toostabilize and for validation toosucceed.</span></span> <span data-ttu-id="e1f92-135">Pokud selhání naleznete v ověření clusteru, Chaos generuje a přetrvává ValidationFailedEvent s časovým razítkem UTC hello a podrobnosti o chybě hello.</span><span class="sxs-lookup"><span data-stu-id="e1f92-135">If a failure is found in cluster validation, Chaos generates and persists a ValidationFailedEvent with hello UTC timestamp and hello failure details.</span></span> <span data-ttu-id="e1f92-136">Představte si třeba instanci Chaos, který je nastavený toorun jednu hodinu s maximálně tři souběžných chyb.</span><span class="sxs-lookup"><span data-stu-id="e1f92-136">For example, consider an instance of Chaos that is set toorun for an hour with a maximum of three concurrent faults.</span></span> <span data-ttu-id="e1f92-137">Chaos indukuje tři chyb a poté ověří hello clusteru stavu.</span><span class="sxs-lookup"><span data-stu-id="e1f92-137">Chaos induces three faults, and then validates hello cluster health.</span></span> <span data-ttu-id="e1f92-138">Prostřednictvím hello předchozí opakuje, předá krok, dokud není explicitně zastaven prostřednictvím hello StopChaosAsync API nebo jednu hodinu.</span><span class="sxs-lookup"><span data-stu-id="e1f92-138">It iterates through hello previous step until it is explicitly stopped through hello StopChaosAsync API or one-hour passes.</span></span> <span data-ttu-id="e1f92-139">Pokud hello cluster se změní na není v pořádku v kteroukoli iteraci (to znamená, že ho není stabilizovat v rámci hello předané MaxClusterStabilizationTimeout), Chaos generuje ValidationFailedEvent.</span><span class="sxs-lookup"><span data-stu-id="e1f92-139">If hello cluster becomes unhealthy in any iteration (that is, it does not stabilize within hello passed-in MaxClusterStabilizationTimeout), Chaos generates a ValidationFailedEvent.</span></span> <span data-ttu-id="e1f92-140">Tato událost označuje, že něco nepovede a může být třeba další šetření.</span><span class="sxs-lookup"><span data-stu-id="e1f92-140">This event indicates that something has gone wrong and might need further investigation.</span></span>

<span data-ttu-id="e1f92-141">tooget, který závady Chaos vyvolané, můžete použít rozhraní API GetChaosReport (prostředí powershell nebo C#).</span><span class="sxs-lookup"><span data-stu-id="e1f92-141">tooget which faults Chaos induced, you can use GetChaosReport API (powershell or C#).</span></span> <span data-ttu-id="e1f92-142">Hello API získá další segment hello hello Chaos sestavy na základě token pokračování předané hello nebo hello předané čas range.</span><span class="sxs-lookup"><span data-stu-id="e1f92-142">hello API gets hello next segment of hello Chaos report based on hello passed-in continuation token or hello passed-in time-range.</span></span> <span data-ttu-id="e1f92-143">Můžete zadat hello ContinuationToken tooget hello další segment hello Chaos sestavy nebo můžete zadat časový rozsah hello prostřednictvím StartTimeUtc a EndTimeUtc, nemůžete ale zadat hello ContinuationToken i hello období v hello stejné volání.</span><span class="sxs-lookup"><span data-stu-id="e1f92-143">You can either specify hello ContinuationToken tooget hello next segment of hello Chaos report or you can specify hello time-range through StartTimeUtc and EndTimeUtc, but you cannot specify both hello ContinuationToken and hello time-range in hello same call.</span></span> <span data-ttu-id="e1f92-144">Když nejsou k dispozici více než 100 Chaos události, hello Chaos sestavy se vrátí v segmentech, kde segment obsahuje více než 100 Chaos událostí.</span><span class="sxs-lookup"><span data-stu-id="e1f92-144">When there are more than 100 Chaos events, hello Chaos report is returned in segments where a segment contains no more than 100 Chaos events.</span></span>

## <a name="important-configuration-options"></a><span data-ttu-id="e1f92-145">Důležité konfigurační možnosti</span><span class="sxs-lookup"><span data-stu-id="e1f92-145">Important configuration options</span></span>
* <span data-ttu-id="e1f92-146">**TimeToRun**: celkový čas která Chaos spouští před dokončením s úspěch.</span><span class="sxs-lookup"><span data-stu-id="e1f92-146">**TimeToRun**: Total time that Chaos runs before it finishes with success.</span></span> <span data-ttu-id="e1f92-147">Můžete zastavit Chaos předtím, než byl spuštěn po dobu TimeToRun hello prostřednictvím hello StopChaos rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="e1f92-147">You can stop Chaos before it has run for hello TimeToRun period through hello StopChaos API.</span></span>

* <span data-ttu-id="e1f92-148">**MaxClusterStabilizationTimeout**: hello maximální množství času toowait pro toobecome clusteru hello před generovala ValidationFailedEvent v pořádku.</span><span class="sxs-lookup"><span data-stu-id="e1f92-148">**MaxClusterStabilizationTimeout**: hello maximum amount of time toowait for hello cluster toobecome healthy before producing a ValidationFailedEvent.</span></span> <span data-ttu-id="e1f92-149">Toto čekání je tooreduce hello zatížení v clusteru hello, zatímco probíhá obnovení.</span><span class="sxs-lookup"><span data-stu-id="e1f92-149">This wait is tooreduce hello load on hello cluster while it is recovering.</span></span> <span data-ttu-id="e1f92-150">provést kontroly Hello jsou:</span><span class="sxs-lookup"><span data-stu-id="e1f92-150">hello checks performed are:</span></span>
  * <span data-ttu-id="e1f92-151">Pokud stav clusteru hello je v pořádku</span><span class="sxs-lookup"><span data-stu-id="e1f92-151">If hello cluster health is OK</span></span>
  * <span data-ttu-id="e1f92-152">Pokud stav služby hello je v pořádku.</span><span class="sxs-lookup"><span data-stu-id="e1f92-152">If hello service health is OK</span></span>
  * <span data-ttu-id="e1f92-153">Pokud cílový repliky hello nastavit velikost se dá dosáhnout oddílu služby hello</span><span class="sxs-lookup"><span data-stu-id="e1f92-153">If hello target replica set size is achieved for hello service partition</span></span>
  * <span data-ttu-id="e1f92-154">Že neexistuje žádná replik InBuild</span><span class="sxs-lookup"><span data-stu-id="e1f92-154">That no InBuild replicas exist</span></span>
* <span data-ttu-id="e1f92-155">**MaxConcurrentFaults**: maximální počet souběžných chyb, které jsou v každé iteraci vyvolané hello.</span><span class="sxs-lookup"><span data-stu-id="e1f92-155">**MaxConcurrentFaults**: hello maximum number of concurrent faults that are induced in each iteration.</span></span> <span data-ttu-id="e1f92-156">vyšší číslo hello Hello, hello agresivnější je Chaos a prochází hello převzetí služeb při selhání a hello stavu přechodu kombinace, které hello clusteru jsou i složitější.</span><span class="sxs-lookup"><span data-stu-id="e1f92-156">hello higher hello number, hello more aggressive Chaos is and hello failovers and hello state transition combinations that hello cluster goes through are also more complex.</span></span> 

> [!NOTE]
> <span data-ttu-id="e1f92-157">Bez ohledu na to jak velkou hodnotu *MaxConcurrentFaults* má Chaos zaručuje - hello neexistence externí chyb - neexistuje ztrátě dat nebo ztrátě kvora.</span><span class="sxs-lookup"><span data-stu-id="e1f92-157">Regardless how high a value *MaxConcurrentFaults* has, Chaos guarantees - in hello absence of external faults - there is no quorum loss or data loss.</span></span>
>

* <span data-ttu-id="e1f92-158">**EnableMoveReplicaFaults**: Povolí nebo zakáže hello chyb, které způsobují toomove hello primární nebo sekundární repliky.</span><span class="sxs-lookup"><span data-stu-id="e1f92-158">**EnableMoveReplicaFaults**: Enables or disables hello faults that cause hello primary or secondary replicas toomove.</span></span> <span data-ttu-id="e1f92-159">Tyto chyby jsou ve výchozím nastavení zakázány.</span><span class="sxs-lookup"><span data-stu-id="e1f92-159">These faults are disabled by default.</span></span>
* <span data-ttu-id="e1f92-160">**WaitTimeBetweenIterations**: hello množství času toowait mezi iterací.</span><span class="sxs-lookup"><span data-stu-id="e1f92-160">**WaitTimeBetweenIterations**: hello amount of time toowait between iterations.</span></span> <span data-ttu-id="e1f92-161">To znamená hello množství času, které Chaos se pozastaví po provedení zaokrouhlit chyb a nutnosti dokončení hello odpovídající ověření stavu hello hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="e1f92-161">That is, hello amount of time Chaos will pause after having executed a round of faults and having finished hello corresponding validation of hello health of hello cluster.</span></span> <span data-ttu-id="e1f92-162">vyšší hodnota hello Hello hello nižší je míra vkládání průměrná selhání hello.</span><span class="sxs-lookup"><span data-stu-id="e1f92-162">hello higher hello value, hello lower is hello average fault injection rate.</span></span>
* <span data-ttu-id="e1f92-163">**WaitTimeBetweenFaults**: hello množství času toowait mezi dvě po sobě jdoucích chyb v jednom iterace.</span><span class="sxs-lookup"><span data-stu-id="e1f92-163">**WaitTimeBetweenFaults**: hello amount of time toowait between two consecutive faults in a single iteration.</span></span> <span data-ttu-id="e1f92-164">Hello vyšší hodnota hello, hello nižší shodu hello (nebo hello překryv mezi) chyb.</span><span class="sxs-lookup"><span data-stu-id="e1f92-164">hello higher hello value, hello lower hello concurrency of (or hello overlap between) faults.</span></span>
* <span data-ttu-id="e1f92-165">**ClusterHealthPolicy**: zásady stavu clusteru je použité toovalidate hello stavu hello clusteru mezi Chaos iterací.</span><span class="sxs-lookup"><span data-stu-id="e1f92-165">**ClusterHealthPolicy**: Cluster health policy is used toovalidate hello health of hello cluster in between Chaos iterations.</span></span> <span data-ttu-id="e1f92-166">Pokud hello stav clusteru došlo k chybě nebo pokud se stane neočekávané výjimce během zpracování chyby, budou Chaos Počkejte 30 minut před hello další – kontrola stavu - tooprovide hello cluster s některé toorecuperate čas.</span><span class="sxs-lookup"><span data-stu-id="e1f92-166">If hello cluster health is in error or if an unexpected exception happens during fault execution, Chaos will wait for 30 minutes before hello next health-check - tooprovide hello cluster with some time toorecuperate.</span></span>
* <span data-ttu-id="e1f92-167">**Kontext**: kolekce (řetězec, řetězec) zadejte páry klíč hodnota.</span><span class="sxs-lookup"><span data-stu-id="e1f92-167">**Context**: A collection of (string, string) type key-value pairs.</span></span> <span data-ttu-id="e1f92-168">Mapa Hello může být použité toorecord informace o hello Chaos spustit.</span><span class="sxs-lookup"><span data-stu-id="e1f92-168">hello map can be used toorecord information about hello Chaos run.</span></span> <span data-ttu-id="e1f92-169">Nemůže být více než 100 tyto dvojice a každý řetězec (klíč nebo hodnota) může mít maximálně povolených 4095 znaků dlouhé.</span><span class="sxs-lookup"><span data-stu-id="e1f92-169">There cannot be more than 100 such pairs and each string (key or value) can be at most 4095 characters long.</span></span> <span data-ttu-id="e1f92-170">Tato mapa je nastavena podle hello starter hello Chaos spustit toooptionally úložiště hello kontextu o hello konkrétní spustit.</span><span class="sxs-lookup"><span data-stu-id="e1f92-170">This map is set by hello starter of hello Chaos run toooptionally store hello context about hello specific run.</span></span>

## <a name="how-toorun-chaos"></a><span data-ttu-id="e1f92-171">Jak toorun Chaos</span><span class="sxs-lookup"><span data-stu-id="e1f92-171">How toorun Chaos</span></span>

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
                Console.WriteLine("An instance of Chaos is already running in hello cluster.");
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
                // If a StoppedEvent is found, exit hello loop.
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
