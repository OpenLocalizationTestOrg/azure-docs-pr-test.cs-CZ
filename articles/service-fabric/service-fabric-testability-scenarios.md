---
title: "Vytváření testů chaos a převzetí služeb při selhání pro Azure mikroslužeb | Microsoft Docs"
description: "Pomocí Service Fabric chaos test a převzetí služeb při selhání testovací scénáře vyvolat chyb a ověřte spolehlivost vašich služeb."
services: service-fabric
documentationcenter: .net
author: motanv
manager: rsinha
editor: toddabel
ms.assetid: 8eee7e89-404a-4605-8f00-7e4d4fb17553
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/07/2017
ms.author: motanv
ms.openlocfilehash: d06026c750e01ad5825338a78d9af331265f434a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="testability-scenarios"></a><span data-ttu-id="c7887-103">Testovatelnosti scénáře</span><span class="sxs-lookup"><span data-stu-id="c7887-103">Testability scenarios</span></span>
<span data-ttu-id="c7887-104">Velkých distribuovaných systémech stejně, jako jsou ze své podstaty nespolehlivé cloudových infrastruktur.</span><span class="sxs-lookup"><span data-stu-id="c7887-104">Large distributed systems like cloud infrastructures are inherently unreliable.</span></span> <span data-ttu-id="c7887-105">Azure Service Fabric nabízí vývojářům možnost zapisovat spuštění nad nespolehlivé infrastruktury služeb.</span><span class="sxs-lookup"><span data-stu-id="c7887-105">Azure Service Fabric gives developers the ability to write services to run on top of unreliable infrastructures.</span></span> <span data-ttu-id="c7887-106">Chcete-li potřebovat vývojáři mohli vyvolat takovou nespolehlivé infrastrukturu k testování stabilitu své služby zápisu vysoce kvalitních služeb.</span><span class="sxs-lookup"><span data-stu-id="c7887-106">In order to write high-quality services, developers need to be able to induce such unreliable infrastructure to test the stability of their services.</span></span>

<span data-ttu-id="c7887-107">Služba Analysis Service odolnost poskytuje vývojářům možnost způsobit selhání akce pro testování služeb v případě selhání.</span><span class="sxs-lookup"><span data-stu-id="c7887-107">The Fault Analysis Service gives developers the ability to induce fault actions to test services in the presence of failures.</span></span> <span data-ttu-id="c7887-108">Ale cílové simulované chyb získáte pouze, pokud.</span><span class="sxs-lookup"><span data-stu-id="c7887-108">However, targeted simulated faults will get you only so far.</span></span> <span data-ttu-id="c7887-109">Chcete-li provádět testování další, můžete použít testovací scénáře v Service Fabric: testovací chaos a testovací převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="c7887-109">To take the testing further, you can use the test scenarios in Service Fabric: a chaos test and a failover test.</span></span> <span data-ttu-id="c7887-110">Tyto scénáře simulovat průběžné prokládaná chyb, řádně a vynuceném v rámci clusteru přes dlouhou dobu.</span><span class="sxs-lookup"><span data-stu-id="c7887-110">These scenarios simulate continuous interleaved faults, both graceful and ungraceful, throughout the cluster over extended periods of time.</span></span> <span data-ttu-id="c7887-111">Jakmile testu je nakonfigurovaný s rychlost a druh chyb, může být spuštěn prostřednictvím rozhraní API jazyka C# nebo prostředí PowerShell ke generování chyb v clusteru a služby.</span><span class="sxs-lookup"><span data-stu-id="c7887-111">Once a test is configured with the rate and kind of faults, it can be started through either C# APIs or PowerShell, to generate faults in the cluster and your service.</span></span>

> [!WARNING]
> <span data-ttu-id="c7887-112">ChaosTestScenario je nahrazován Chaos pružnější, na základě služeb.</span><span class="sxs-lookup"><span data-stu-id="c7887-112">ChaosTestScenario is being replaced by a more resilient, service-based Chaos.</span></span> <span data-ttu-id="c7887-113">Přečtěte nový článek na [řídí Chaos](service-fabric-controlled-chaos.md) další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="c7887-113">Please refer to the new article [Controlled Chaos](service-fabric-controlled-chaos.md) for more details.</span></span>
> 
> 

## <a name="chaos-test"></a><span data-ttu-id="c7887-114">Chaos testu</span><span class="sxs-lookup"><span data-stu-id="c7887-114">Chaos test</span></span>
<span data-ttu-id="c7887-115">Scénář chaos generuje chyby napříč celý cluster Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="c7887-115">The chaos scenario generates faults across the entire Service Fabric cluster.</span></span> <span data-ttu-id="c7887-116">Tento scénář komprimaci chyb obecně zobrazená v měsíců nebo let několik hodin.</span><span class="sxs-lookup"><span data-stu-id="c7887-116">The scenario compresses faults generally seen in months or years to a few hours.</span></span> <span data-ttu-id="c7887-117">Kombinace prokládaná chyb s vysokou odolnost rychlost vyhledá náročnějších případech, které jsou jinak vynechat.</span><span class="sxs-lookup"><span data-stu-id="c7887-117">The combination of interleaved faults with the high fault rate finds corner cases that are otherwise missed.</span></span> <span data-ttu-id="c7887-118">To vede k významné zlepšení kvality kódu služby.</span><span class="sxs-lookup"><span data-stu-id="c7887-118">This leads to a significant improvement in the code quality of the service.</span></span>

### <a name="faults-simulated-in-the-chaos-test"></a><span data-ttu-id="c7887-119">Simulated v testu chaos chyb</span><span class="sxs-lookup"><span data-stu-id="c7887-119">Faults simulated in the chaos test</span></span>
* <span data-ttu-id="c7887-120">Restartovat uzel</span><span class="sxs-lookup"><span data-stu-id="c7887-120">Restart a node</span></span>
* <span data-ttu-id="c7887-121">Restartujte nasazený balíček kódu</span><span class="sxs-lookup"><span data-stu-id="c7887-121">Restart a deployed code package</span></span>
* <span data-ttu-id="c7887-122">Odstranění repliky</span><span class="sxs-lookup"><span data-stu-id="c7887-122">Remove a replica</span></span>
* <span data-ttu-id="c7887-123">Restartujte repliky</span><span class="sxs-lookup"><span data-stu-id="c7887-123">Restart a replica</span></span>
* <span data-ttu-id="c7887-124">Přesunutí primární repliky (volitelné)</span><span class="sxs-lookup"><span data-stu-id="c7887-124">Move a primary replica (optional)</span></span>
* <span data-ttu-id="c7887-125">Přesunutí sekundární repliky (volitelné)</span><span class="sxs-lookup"><span data-stu-id="c7887-125">Move a secondary replica (optional)</span></span>

<span data-ttu-id="c7887-126">Chaos testovací běhy více iterací chyb a ověření clusteru pro zadané časové období.</span><span class="sxs-lookup"><span data-stu-id="c7887-126">The chaos test runs multiple iterations of faults and cluster validations for the specified period of time.</span></span> <span data-ttu-id="c7887-127">Čas strávený clusteru stabilizovat a pro ověření, který má být úspěšné, je také možné konfigurovat.</span><span class="sxs-lookup"><span data-stu-id="c7887-127">The time spent for the cluster to stabilize and for validation to succeed is also configurable.</span></span> <span data-ttu-id="c7887-128">Tento scénář selže, když dosáhl jediné chyby v ověření clusteru.</span><span class="sxs-lookup"><span data-stu-id="c7887-128">The scenario fails when you hit a single failure in cluster validation.</span></span>

<span data-ttu-id="c7887-129">Představte si třeba testu nastaven na spouštění hodinu s maximálně tři souběžných chyb.</span><span class="sxs-lookup"><span data-stu-id="c7887-129">For example, consider a test set to run for one hour with a maximum of three concurrent faults.</span></span> <span data-ttu-id="c7887-130">Test se vyvolat tři chyb a pak ověřte stav clusteru.</span><span class="sxs-lookup"><span data-stu-id="c7887-130">The test will induce three faults, and then validate the cluster health.</span></span> <span data-ttu-id="c7887-131">Test bude iterovat v předchozím kroku, dokud clusteru se změní na není v pořádku nebo předá jednu hodinu.</span><span class="sxs-lookup"><span data-stu-id="c7887-131">The test will iterate through the previous step till the cluster becomes unhealthy or one hour passes.</span></span> <span data-ttu-id="c7887-132">Pokud cluster se změní na není v pořádku v kteroukoli iteraci, tj. není stabilizaci v nakonfigurovaném čase, test se nezdaří s výjimkou.</span><span class="sxs-lookup"><span data-stu-id="c7887-132">If the cluster becomes unhealthy in any iteration, i.e. it does not stabilize within a configured time, the test will fail with an exception.</span></span> <span data-ttu-id="c7887-133">Tato výjimka označuje, že něco nepovede a potřebuje další šetření.</span><span class="sxs-lookup"><span data-stu-id="c7887-133">This exception indicates that something has gone wrong and needs further investigation.</span></span>

<span data-ttu-id="c7887-134">V současné podobě modul selhání generace v testu chaos indukuje pouze bezpečné chyb.</span><span class="sxs-lookup"><span data-stu-id="c7887-134">In its current form, the fault generation engine in the chaos test induces only safe faults.</span></span> <span data-ttu-id="c7887-135">To znamená, že chybí externí chyb, ke ztrátě kvora nebo data nikdy nedojde.</span><span class="sxs-lookup"><span data-stu-id="c7887-135">This means that in the absence of external faults, a quorum or data loss will never occur.</span></span>

### <a name="important-configuration-options"></a><span data-ttu-id="c7887-136">Důležité konfigurační možnosti</span><span class="sxs-lookup"><span data-stu-id="c7887-136">Important configuration options</span></span>
* <span data-ttu-id="c7887-137">**TimeToRun**: celkový čas, který se test spustí před dokončením instalace se.</span><span class="sxs-lookup"><span data-stu-id="c7887-137">**TimeToRun**: Total time that the test will run before finishing with success.</span></span> <span data-ttu-id="c7887-138">Test můžete dokončit dříve místo selhání ověření.</span><span class="sxs-lookup"><span data-stu-id="c7887-138">The test can finish earlier in lieu of a validation failure.</span></span>
* <span data-ttu-id="c7887-139">**MaxClusterStabilizationTimeout**: maximální množství času čekání na clusteru se nezotavila před selháním test.</span><span class="sxs-lookup"><span data-stu-id="c7887-139">**MaxClusterStabilizationTimeout**: Maximum amount of time to wait for the cluster to become healthy before failing the test.</span></span> <span data-ttu-id="c7887-140">Provést kontroly se, zda stav clusteru je v pořádku, stav služby je v pořádku, oddílu služby se dá dosáhnout velikost cílové sady replik a neexistují žádné replik InBuild.</span><span class="sxs-lookup"><span data-stu-id="c7887-140">The checks performed are whether cluster health is OK, service health is OK, the target replica set size is achieved for the service partition, and no InBuild replicas exist.</span></span>
* <span data-ttu-id="c7887-141">**MaxConcurrentFaults**: maximální počet souběžných chyb vyvolané v každé iteraci.</span><span class="sxs-lookup"><span data-stu-id="c7887-141">**MaxConcurrentFaults**: Maximum number of concurrent faults induced in each iteration.</span></span> <span data-ttu-id="c7887-142">Čím vyšší číslo, agresivnější test, proto výsledkem složitější převzetí služeb při selhání a kombinace přechodu.</span><span class="sxs-lookup"><span data-stu-id="c7887-142">The higher the number, the more aggressive the test, hence resulting in more complex failovers and transition combinations.</span></span> <span data-ttu-id="c7887-143">Test zaručuje, že neexistence externí chyb nebude existovat kvora nebo ztráty dat, bez ohledu na to, jak vysoké tato konfigurace je.</span><span class="sxs-lookup"><span data-stu-id="c7887-143">The test guarantees that in absence of external faults there will not be a quorum or data loss, irrespective of how high this configuration is.</span></span>
* <span data-ttu-id="c7887-144">**EnableMoveReplicaFaults**: Povolí nebo zakáže chyb, které způsobují přesun primární nebo sekundární repliky.</span><span class="sxs-lookup"><span data-stu-id="c7887-144">**EnableMoveReplicaFaults**: Enables or disables the faults that are causing the move of the primary or secondary replicas.</span></span> <span data-ttu-id="c7887-145">Tyto chyby jsou ve výchozím nastavení zakázány.</span><span class="sxs-lookup"><span data-stu-id="c7887-145">These faults are disabled by default.</span></span>
* <span data-ttu-id="c7887-146">**WaitTimeBetweenIterations**: dobu čekání mezi iterací, např. po zaokrouhlit chyb a odpovídající ověření.</span><span class="sxs-lookup"><span data-stu-id="c7887-146">**WaitTimeBetweenIterations**: Amount of time to wait between iterations, i.e. after a round of faults and corresponding validation.</span></span>

### <a name="how-to-run-the-chaos-test"></a><span data-ttu-id="c7887-147">Postup spuštění testu chaos</span><span class="sxs-lookup"><span data-stu-id="c7887-147">How to run the chaos test</span></span>
<span data-ttu-id="c7887-148">Ukázka v jazyce C#</span><span class="sxs-lookup"><span data-stu-id="c7887-148">C# sample</span></span>

```csharp
using System;
using System.Fabric;
using System.Fabric.Testability.Scenario;
using System.Threading;
using System.Threading.Tasks;

class Test
{
    public static int Main(string[] args)
    {
        string clusterConnection = "localhost:19000";

        Console.WriteLine("Starting Chaos Test Scenario...");
        try
        {
            RunChaosTestScenarioAsync(clusterConnection).Wait();
        }
        catch (AggregateException ae)
        {
            Console.WriteLine("Chaos Test Scenario did not complete: ");
            foreach (Exception ex in ae.InnerExceptions)
            {
                if (ex is FabricException)
                {
                    Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
                }
            }
            return -1;
        }

        Console.WriteLine("Chaos Test Scenario completed.");
        return 0;
    }

    static async Task RunChaosTestScenarioAsync(string clusterConnection)
    {
        TimeSpan maxClusterStabilizationTimeout = TimeSpan.FromSeconds(180);
        uint maxConcurrentFaults = 3;
        bool enableMoveReplicaFaults = true;

        // Create FabricClient with connection and security information here.
        FabricClient fabricClient = new FabricClient(clusterConnection);

        // The chaos test scenario should run at least 60 minutes or until it fails.
        TimeSpan timeToRun = TimeSpan.FromMinutes(60);
        ChaosTestScenarioParameters scenarioParameters = new ChaosTestScenarioParameters(
          maxClusterStabilizationTimeout,
          maxConcurrentFaults,
          enableMoveReplicaFaults,
          timeToRun);

        // Other related parameters:
        // Pause between two iterations for a random duration bound by this value.
        // scenarioParameters.WaitTimeBetweenIterations = TimeSpan.FromSeconds(30);
        // Pause between concurrent actions for a random duration bound by this value.
        // scenarioParameters.WaitTimeBetweenFaults = TimeSpan.FromSeconds(10);

        // Create the scenario class and execute it asynchronously.
        ChaosTestScenario chaosScenario = new ChaosTestScenario(fabricClient, scenarioParameters);

        try
        {
            await chaosScenario.ExecuteAsync(CancellationToken.None);
        }
        catch (AggregateException ae)
        {
            throw ae.InnerException;
        }
    }
}
```

<span data-ttu-id="c7887-149">PowerShell</span><span class="sxs-lookup"><span data-stu-id="c7887-149">PowerShell</span></span>

```powershell
$connection = "localhost:19000"
$timeToRun = 60
$maxStabilizationTimeSecs = 180
$concurrentFaults = 3
$waitTimeBetweenIterationsSec = 60

Connect-ServiceFabricCluster $connection

Invoke-ServiceFabricChaosTestScenario -TimeToRunMinute $timeToRun -MaxClusterStabilizationTimeoutSec $maxStabilizationTimeSecs -MaxConcurrentFaults $concurrentFaults -EnableMoveReplicaFaults -WaitTimeBetweenIterationsSec $waitTimeBetweenIterationsSec
```


## <a name="failover-test"></a><span data-ttu-id="c7887-150">Testovací převzetí služeb při selhání</span><span class="sxs-lookup"><span data-stu-id="c7887-150">Failover test</span></span>
<span data-ttu-id="c7887-151">Scénáře testovacího převzetí služeb při selhání je verze chaos testovací scénář, který cílí na konkrétní službu oddíl.</span><span class="sxs-lookup"><span data-stu-id="c7887-151">The failover test scenario is a version of the chaos test scenario that targets a specific service partition.</span></span> <span data-ttu-id="c7887-152">Testy vliv převzetí služeb při selhání na oddíl konkrétní službu, a nechat neovlivní jiným službám.</span><span class="sxs-lookup"><span data-stu-id="c7887-152">It tests the effect of failover on a specific service partition while leaving the other services unaffected.</span></span> <span data-ttu-id="c7887-153">Jakmile je nakonfigurován s informace o cílové oddílu a dalších parametrů, spouští se jako nástroj na straně klienta, který používá rozhraní API jazyka C# nebo prostředí PowerShell ke generování chyb pro oddíl služby.</span><span class="sxs-lookup"><span data-stu-id="c7887-153">Once it's configured with the target partition information and other parameters, it runs as a client-side tool that uses either C# APIs or PowerShell to generate faults for a service partition.</span></span> <span data-ttu-id="c7887-154">Tento scénář iteruje posloupnost simulované chyb a ověření služby průběhu obchodní logiky na straně zajistit zatížení.</span><span class="sxs-lookup"><span data-stu-id="c7887-154">The scenario iterates through a sequence of simulated faults and service validation while your business logic runs on the side to provide a workload.</span></span> <span data-ttu-id="c7887-155">Chyby při ověřování služby označuje potíže, které potřebuje další šetření.</span><span class="sxs-lookup"><span data-stu-id="c7887-155">A failure in service validation indicates an issue that needs further investigation.</span></span>

### <a name="faults-simulated-in-the-failover-test"></a><span data-ttu-id="c7887-156">Chyby simulated v testu převzetí služeb při selhání</span><span class="sxs-lookup"><span data-stu-id="c7887-156">Faults simulated in the failover test</span></span>
* <span data-ttu-id="c7887-157">Restartujte nasazený balíček kódu je hostitelem oddílu</span><span class="sxs-lookup"><span data-stu-id="c7887-157">Restart a deployed code package where the partition is hosted</span></span>
* <span data-ttu-id="c7887-158">Odebrat primární a sekundární repliky nebo bezstavové instance</span><span class="sxs-lookup"><span data-stu-id="c7887-158">Remove a primary/secondary replica or stateless instance</span></span>
* <span data-ttu-id="c7887-159">Restartujte primární sekundární repliky (Pokud trvalou služba)</span><span class="sxs-lookup"><span data-stu-id="c7887-159">Restart a primary secondary replica (if a persisted service)</span></span>
* <span data-ttu-id="c7887-160">Přesunutí primární repliky</span><span class="sxs-lookup"><span data-stu-id="c7887-160">Move a primary replica</span></span>
* <span data-ttu-id="c7887-161">Přesunutí sekundární repliky</span><span class="sxs-lookup"><span data-stu-id="c7887-161">Move a secondary replica</span></span>
* <span data-ttu-id="c7887-162">Restartujte oddílu</span><span class="sxs-lookup"><span data-stu-id="c7887-162">Restart the partition</span></span>

<span data-ttu-id="c7887-163">Testovací převzetí služeb při selhání indukuje zvolené chybu a pak spustí ověřování na službě, abyste zajistili její stability.</span><span class="sxs-lookup"><span data-stu-id="c7887-163">The failover test induces a chosen fault and then runs validation on the service to ensure its stability.</span></span> <span data-ttu-id="c7887-164">Testovací převzetí služeb při selhání indukuje pouze jeden selhání současně, a možné několik chyb v chaos test.</span><span class="sxs-lookup"><span data-stu-id="c7887-164">The failover test induces only one fault at a time, as opposed to possible multiple faults in the chaos test.</span></span> <span data-ttu-id="c7887-165">Pokud služba oddílu není stabilizaci s nakonfigurovaným časovým limitem po každé selhání, test se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="c7887-165">If the service partition does not stabilize within the configured timeout after each fault, the test fails.</span></span> <span data-ttu-id="c7887-166">Test indukuje pouze bezpečné chyb.</span><span class="sxs-lookup"><span data-stu-id="c7887-166">The test induces only safe faults.</span></span> <span data-ttu-id="c7887-167">To znamená, existovat externí selhání, nedojde ke ztrátě kvora nebo data.</span><span class="sxs-lookup"><span data-stu-id="c7887-167">This means that in absence of external failures, a quorum or data loss will not occur.</span></span>

### <a name="important-configuration-options"></a><span data-ttu-id="c7887-168">Důležité konfigurační možnosti</span><span class="sxs-lookup"><span data-stu-id="c7887-168">Important configuration options</span></span>
* <span data-ttu-id="c7887-169">**Partitionselector nejde**: výběr objektu, který určuje oddílu, který musí být cílem.</span><span class="sxs-lookup"><span data-stu-id="c7887-169">**PartitionSelector**: Selector object that specifies the partition that needs to be targeted.</span></span>
* <span data-ttu-id="c7887-170">**TimeToRun**: celkový čas, který se test spustí před dokončením instalace.</span><span class="sxs-lookup"><span data-stu-id="c7887-170">**TimeToRun**: Total time that the test will run before finishing.</span></span>
* <span data-ttu-id="c7887-171">**MaxServiceStabilizationTimeout**: maximální množství času čekání na clusteru se nezotavila před selháním test.</span><span class="sxs-lookup"><span data-stu-id="c7887-171">**MaxServiceStabilizationTimeout**: Maximum amount of time to wait for the cluster to become healthy before failing the test.</span></span> <span data-ttu-id="c7887-172">Provést kontroly se, jestli stav služby je v pořádku, velikost cílové sady replik je dosaženo pro všechny oddíly a neexistují žádné replik InBuild.</span><span class="sxs-lookup"><span data-stu-id="c7887-172">The checks performed are whether service health is OK, the target replica set size is achieved for all partitions, and no InBuild replicas exist.</span></span>
* <span data-ttu-id="c7887-173">**WaitTimeBetweenFaults**: dobu čekání mezi každý cyklus selhání a ověření.</span><span class="sxs-lookup"><span data-stu-id="c7887-173">**WaitTimeBetweenFaults**: Amount of time to wait between every fault and validation cycle.</span></span>

### <a name="how-to-run-the-failover-test"></a><span data-ttu-id="c7887-174">Postup spuštění testu převzetí služeb při selhání</span><span class="sxs-lookup"><span data-stu-id="c7887-174">How to run the failover test</span></span>
<span data-ttu-id="c7887-175">**C#**</span><span class="sxs-lookup"><span data-stu-id="c7887-175">**C#**</span></span>

```csharp
using System;
using System.Fabric;
using System.Fabric.Testability.Scenario;
using System.Threading;
using System.Threading.Tasks;

class Test
{
    public static int Main(string[] args)
    {
        string clusterConnection = "localhost:19000";
        Uri serviceName = new Uri("fabric:/samples/PersistentToDoListApp/PersistentToDoListService");

        Console.WriteLine("Starting Chaos Test Scenario...");
        try
        {
            RunFailoverTestScenarioAsync(clusterConnection, serviceName).Wait();
        }
        catch (AggregateException ae)
        {
            Console.WriteLine("Chaos Test Scenario did not complete: ");
            foreach (Exception ex in ae.InnerExceptions)
            {
                if (ex is FabricException)
                {
                    Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
                }
            }
            return -1;
        }

        Console.WriteLine("Chaos Test Scenario completed.");
        return 0;
    }

    static async Task RunFailoverTestScenarioAsync(string clusterConnection, Uri serviceName)
    {
        TimeSpan maxServiceStabilizationTimeout = TimeSpan.FromSeconds(180);
        PartitionSelector randomPartitionSelector = PartitionSelector.RandomOf(serviceName);

        // Create FabricClient with connection and security information here.
        FabricClient fabricClient = new FabricClient(clusterConnection);

        // The chaos test scenario should run at least 60 minutes or until it fails.
        TimeSpan timeToRun = TimeSpan.FromMinutes(60);
        FailoverTestScenarioParameters scenarioParameters = new FailoverTestScenarioParameters(
          randomPartitionSelector,
          timeToRun,
          maxServiceStabilizationTimeout);

        // Other related parameters:
        // Pause between two iterations for a random duration bound by this value.
        // scenarioParameters.WaitTimeBetweenIterations = TimeSpan.FromSeconds(30);
        // Pause between concurrent actions for a random duration bound by this value.
        // scenarioParameters.WaitTimeBetweenFaults = TimeSpan.FromSeconds(10);

        // Create the scenario class and execute it asynchronously.
        FailoverTestScenario failoverScenario = new FailoverTestScenario(fabricClient, scenarioParameters);

        try
        {
            await failoverScenario.ExecuteAsync(CancellationToken.None);
        }
        catch (AggregateException ae)
        {
            throw ae.InnerException;
        }
    }
}
```


<span data-ttu-id="c7887-176">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="c7887-176">**PowerShell**</span></span>

```powershell
$connection = "localhost:19000"
$timeToRun = 60
$maxStabilizationTimeSecs = 180
$waitTimeBetweenFaultsSec = 10
$serviceName = "fabric:/SampleApp/SampleService"

Connect-ServiceFabricCluster $connection

Invoke-ServiceFabricFailoverTestScenario -TimeToRunMinute $timeToRun -MaxServiceStabilizationTimeoutSec $maxStabilizationTimeSecs -WaitTimeBetweenFaultsSec $waitTimeBetweenFaultsSec -ServiceName $serviceName -PartitionKindSingleton
```
