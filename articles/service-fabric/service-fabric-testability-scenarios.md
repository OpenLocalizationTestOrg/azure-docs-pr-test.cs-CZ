---
title: "aaaCreate chaos a převzetí služeb při selhání testování pro Azure mikroslužeb | Microsoft Docs"
description: "Pomocí testovacích chaos hello Service Fabric a převzetí služeb při selhání testovací scénáře tooinduce chyb a ověřte spolehlivost hello vašich služeb."
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
ms.openlocfilehash: 1cac4f9e0e4a6c8416d5220d1537b5110decd1f7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="testability-scenarios"></a><span data-ttu-id="9b68c-103">Testovatelnosti scénáře</span><span class="sxs-lookup"><span data-stu-id="9b68c-103">Testability scenarios</span></span>
<span data-ttu-id="9b68c-104">Velkých distribuovaných systémech stejně, jako jsou ze své podstaty nespolehlivé cloudových infrastruktur.</span><span class="sxs-lookup"><span data-stu-id="9b68c-104">Large distributed systems like cloud infrastructures are inherently unreliable.</span></span> <span data-ttu-id="9b68c-105">Azure Service Fabric nabízí vývojářům hello možnost toowrite služby toorun nad nespolehlivé infrastruktury.</span><span class="sxs-lookup"><span data-stu-id="9b68c-105">Azure Service Fabric gives developers hello ability toowrite services toorun on top of unreliable infrastructures.</span></span> <span data-ttu-id="9b68c-106">V pořadí toowrite vysoce kvalitních služeb vývojáři nutné mít tooinduce toobe takové nespolehlivé infrastruktury tootest hello stabilitu své služby.</span><span class="sxs-lookup"><span data-stu-id="9b68c-106">In order toowrite high-quality services, developers need toobe able tooinduce such unreliable infrastructure tootest hello stability of their services.</span></span>

<span data-ttu-id="9b68c-107">Hello selhání Analysis Service poskytuje vývojářům hello možnost tooinduce selhání akce tootest služby v hello přítomnost chyb.</span><span class="sxs-lookup"><span data-stu-id="9b68c-107">hello Fault Analysis Service gives developers hello ability tooinduce fault actions tootest services in hello presence of failures.</span></span> <span data-ttu-id="9b68c-108">Ale cílové simulované chyb získáte pouze, pokud.</span><span class="sxs-lookup"><span data-stu-id="9b68c-108">However, targeted simulated faults will get you only so far.</span></span> <span data-ttu-id="9b68c-109">testování navíc hello tootake hello testovací scénáře můžete použít v Service Fabric: testovací chaos a testovací převzetí služeb při selhání.</span><span class="sxs-lookup"><span data-stu-id="9b68c-109">tootake hello testing further, you can use hello test scenarios in Service Fabric: a chaos test and a failover test.</span></span> <span data-ttu-id="9b68c-110">Tyto scénáře simulovat průběžné prokládaná chyb, řádně a vynuceném v rámci clusteru hello přes dlouhou dobu.</span><span class="sxs-lookup"><span data-stu-id="9b68c-110">These scenarios simulate continuous interleaved faults, both graceful and ungraceful, throughout hello cluster over extended periods of time.</span></span> <span data-ttu-id="9b68c-111">Jakmile testu je nakonfigurovaný s hello rychlost a druh chyb, může být spuštěn prostřednictvím rozhraní API jazyka C# nebo prostředí PowerShell, toogenerate chyb v hello clusteru a služby.</span><span class="sxs-lookup"><span data-stu-id="9b68c-111">Once a test is configured with hello rate and kind of faults, it can be started through either C# APIs or PowerShell, toogenerate faults in hello cluster and your service.</span></span>

> [!WARNING]
> <span data-ttu-id="9b68c-112">ChaosTestScenario je nahrazován Chaos pružnější, na základě služeb.</span><span class="sxs-lookup"><span data-stu-id="9b68c-112">ChaosTestScenario is being replaced by a more resilient, service-based Chaos.</span></span> <span data-ttu-id="9b68c-113">Naleznete v článku nové toohello [řídí Chaos](service-fabric-controlled-chaos.md) další podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="9b68c-113">Please refer toohello new article [Controlled Chaos](service-fabric-controlled-chaos.md) for more details.</span></span>
> 
> 

## <a name="chaos-test"></a><span data-ttu-id="9b68c-114">Chaos testu</span><span class="sxs-lookup"><span data-stu-id="9b68c-114">Chaos test</span></span>
<span data-ttu-id="9b68c-115">scénář chaos Hello generuje chyby napříč hello celý cluster Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="9b68c-115">hello chaos scenario generates faults across hello entire Service Fabric cluster.</span></span> <span data-ttu-id="9b68c-116">scénář Hello komprimaci chyb obecně zobrazená v měsíců nebo let tooa několik hodin.</span><span class="sxs-lookup"><span data-stu-id="9b68c-116">hello scenario compresses faults generally seen in months or years tooa few hours.</span></span> <span data-ttu-id="9b68c-117">kombinace Hello prokládaná chyb hello vysokou odolnost míru vyhledá náročnějších případech, které jsou jinak vynechat.</span><span class="sxs-lookup"><span data-stu-id="9b68c-117">hello combination of interleaved faults with hello high fault rate finds corner cases that are otherwise missed.</span></span> <span data-ttu-id="9b68c-118">To vede tooa výrazné zlepšení kvality kódu hello hello služby.</span><span class="sxs-lookup"><span data-stu-id="9b68c-118">This leads tooa significant improvement in hello code quality of hello service.</span></span>

### <a name="faults-simulated-in-hello-chaos-test"></a><span data-ttu-id="9b68c-119">Chyby simulated v testu chaos hello</span><span class="sxs-lookup"><span data-stu-id="9b68c-119">Faults simulated in hello chaos test</span></span>
* <span data-ttu-id="9b68c-120">Restartovat uzel</span><span class="sxs-lookup"><span data-stu-id="9b68c-120">Restart a node</span></span>
* <span data-ttu-id="9b68c-121">Restartujte nasazený balíček kódu</span><span class="sxs-lookup"><span data-stu-id="9b68c-121">Restart a deployed code package</span></span>
* <span data-ttu-id="9b68c-122">Odstranění repliky</span><span class="sxs-lookup"><span data-stu-id="9b68c-122">Remove a replica</span></span>
* <span data-ttu-id="9b68c-123">Restartujte repliky</span><span class="sxs-lookup"><span data-stu-id="9b68c-123">Restart a replica</span></span>
* <span data-ttu-id="9b68c-124">Přesunutí primární repliky (volitelné)</span><span class="sxs-lookup"><span data-stu-id="9b68c-124">Move a primary replica (optional)</span></span>
* <span data-ttu-id="9b68c-125">Přesunutí sekundární repliky (volitelné)</span><span class="sxs-lookup"><span data-stu-id="9b68c-125">Move a secondary replica (optional)</span></span>

<span data-ttu-id="9b68c-126">Hello chaos testovací běhy více opakování chyb a ověření clusteru pro hello zadané časové období.</span><span class="sxs-lookup"><span data-stu-id="9b68c-126">hello chaos test runs multiple iterations of faults and cluster validations for hello specified period of time.</span></span> <span data-ttu-id="9b68c-127">Hello času stráveného pro toostabilize hello clusteru a pro ověření toosucceed je také možné konfigurovat.</span><span class="sxs-lookup"><span data-stu-id="9b68c-127">hello time spent for hello cluster toostabilize and for validation toosucceed is also configurable.</span></span> <span data-ttu-id="9b68c-128">scénář Hello selže, když dosáhl jediné chyby v ověření clusteru.</span><span class="sxs-lookup"><span data-stu-id="9b68c-128">hello scenario fails when you hit a single failure in cluster validation.</span></span>

<span data-ttu-id="9b68c-129">Zvažte například, že testu toorun nastaven na jednu hodinu s maximálně tři souběžných chyb.</span><span class="sxs-lookup"><span data-stu-id="9b68c-129">For example, consider a test set toorun for one hour with a maximum of three concurrent faults.</span></span> <span data-ttu-id="9b68c-130">Hello test se vyvolat tři chyb a pak ověřte stav clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="9b68c-130">hello test will induce three faults, and then validate hello cluster health.</span></span> <span data-ttu-id="9b68c-131">Hello test bude iteraci v rámci předchozí krok hello dokud hello clusteru se změní na není v pořádku nebo předá jednu hodinu.</span><span class="sxs-lookup"><span data-stu-id="9b68c-131">hello test will iterate through hello previous step till hello cluster becomes unhealthy or one hour passes.</span></span> <span data-ttu-id="9b68c-132">Pokud hello cluster se změní na není v pořádku v kteroukoli iteraci, tj. není stabilizaci v nakonfigurovaném čase, hello test se nezdaří s výjimkou.</span><span class="sxs-lookup"><span data-stu-id="9b68c-132">If hello cluster becomes unhealthy in any iteration, i.e. it does not stabilize within a configured time, hello test will fail with an exception.</span></span> <span data-ttu-id="9b68c-133">Tato výjimka označuje, že něco nepovede a potřebuje další šetření.</span><span class="sxs-lookup"><span data-stu-id="9b68c-133">This exception indicates that something has gone wrong and needs further investigation.</span></span>

<span data-ttu-id="9b68c-134">V současné podobě indukuje hello selhání generování modul v testu chaos hello pouze bezpečné chyb.</span><span class="sxs-lookup"><span data-stu-id="9b68c-134">In its current form, hello fault generation engine in hello chaos test induces only safe faults.</span></span> <span data-ttu-id="9b68c-135">To znamená, že v hello chybí externí chyb, ke ztrátě kvora nebo data nikdy nedojde.</span><span class="sxs-lookup"><span data-stu-id="9b68c-135">This means that in hello absence of external faults, a quorum or data loss will never occur.</span></span>

### <a name="important-configuration-options"></a><span data-ttu-id="9b68c-136">Důležité konfigurační možnosti</span><span class="sxs-lookup"><span data-stu-id="9b68c-136">Important configuration options</span></span>
* <span data-ttu-id="9b68c-137">**TimeToRun**: celkový čas testu hello bude spuštěn před dokončením s úspěch.</span><span class="sxs-lookup"><span data-stu-id="9b68c-137">**TimeToRun**: Total time that hello test will run before finishing with success.</span></span> <span data-ttu-id="9b68c-138">Hello test můžete dokončit dříve místo selhání ověření.</span><span class="sxs-lookup"><span data-stu-id="9b68c-138">hello test can finish earlier in lieu of a validation failure.</span></span>
* <span data-ttu-id="9b68c-139">**MaxClusterStabilizationTimeout**: maximální množství času toowait pro toobecome clusteru hello před selháním hello testu v pořádku.</span><span class="sxs-lookup"><span data-stu-id="9b68c-139">**MaxClusterStabilizationTimeout**: Maximum amount of time toowait for hello cluster toobecome healthy before failing hello test.</span></span> <span data-ttu-id="9b68c-140">Hello provést kontroly jsou jestli stav clusteru je v pořádku, stav služby je v pořádku, hello velikost cílové sady replik se dá dosáhnout hello oddílu služby a neexistují žádné replik InBuild.</span><span class="sxs-lookup"><span data-stu-id="9b68c-140">hello checks performed are whether cluster health is OK, service health is OK, hello target replica set size is achieved for hello service partition, and no InBuild replicas exist.</span></span>
* <span data-ttu-id="9b68c-141">**MaxConcurrentFaults**: maximální počet souběžných chyb vyvolané v každé iteraci.</span><span class="sxs-lookup"><span data-stu-id="9b68c-141">**MaxConcurrentFaults**: Maximum number of concurrent faults induced in each iteration.</span></span> <span data-ttu-id="9b68c-142">Dobrý den vyšší číslo hello, hello agresivnější hello test, proto výsledkem složitější převzetí služeb při selhání a kombinace přechodu.</span><span class="sxs-lookup"><span data-stu-id="9b68c-142">hello higher hello number, hello more aggressive hello test, hence resulting in more complex failovers and transition combinations.</span></span> <span data-ttu-id="9b68c-143">Hello test zaručuje, že neexistence externí chyb nebude existovat kvora nebo ztráty dat, bez ohledu na to, jak vysoké tato konfigurace je.</span><span class="sxs-lookup"><span data-stu-id="9b68c-143">hello test guarantees that in absence of external faults there will not be a quorum or data loss, irrespective of how high this configuration is.</span></span>
* <span data-ttu-id="9b68c-144">**EnableMoveReplicaFaults**: Povolí nebo zakáže hello chyb, které způsobují hello přesun hello primární nebo sekundární repliky.</span><span class="sxs-lookup"><span data-stu-id="9b68c-144">**EnableMoveReplicaFaults**: Enables or disables hello faults that are causing hello move of hello primary or secondary replicas.</span></span> <span data-ttu-id="9b68c-145">Tyto chyby jsou ve výchozím nastavení zakázány.</span><span class="sxs-lookup"><span data-stu-id="9b68c-145">These faults are disabled by default.</span></span>
* <span data-ttu-id="9b68c-146">**WaitTimeBetweenIterations**: množství času toowait mezi iterací, např. po zaokrouhlit chyb a odpovídající ověření.</span><span class="sxs-lookup"><span data-stu-id="9b68c-146">**WaitTimeBetweenIterations**: Amount of time toowait between iterations, i.e. after a round of faults and corresponding validation.</span></span>

### <a name="how-toorun-hello-chaos-test"></a><span data-ttu-id="9b68c-147">Jak toorun hello chaos testování</span><span class="sxs-lookup"><span data-stu-id="9b68c-147">How toorun hello chaos test</span></span>
<span data-ttu-id="9b68c-148">Ukázka v jazyce C#</span><span class="sxs-lookup"><span data-stu-id="9b68c-148">C# sample</span></span>

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

        // hello chaos test scenario should run at least 60 minutes or until it fails.
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

        // Create hello scenario class and execute it asynchronously.
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

<span data-ttu-id="9b68c-149">PowerShell</span><span class="sxs-lookup"><span data-stu-id="9b68c-149">PowerShell</span></span>

```powershell
$connection = "localhost:19000"
$timeToRun = 60
$maxStabilizationTimeSecs = 180
$concurrentFaults = 3
$waitTimeBetweenIterationsSec = 60

Connect-ServiceFabricCluster $connection

Invoke-ServiceFabricChaosTestScenario -TimeToRunMinute $timeToRun -MaxClusterStabilizationTimeoutSec $maxStabilizationTimeSecs -MaxConcurrentFaults $concurrentFaults -EnableMoveReplicaFaults -WaitTimeBetweenIterationsSec $waitTimeBetweenIterationsSec
```


## <a name="failover-test"></a><span data-ttu-id="9b68c-150">Testovací převzetí služeb při selhání</span><span class="sxs-lookup"><span data-stu-id="9b68c-150">Failover test</span></span>
<span data-ttu-id="9b68c-151">scénáře testovacího převzetí služeb při selhání Hello je verze hello chaos testovací scénář, který cílí na konkrétní službu oddíl.</span><span class="sxs-lookup"><span data-stu-id="9b68c-151">hello failover test scenario is a version of hello chaos test scenario that targets a specific service partition.</span></span> <span data-ttu-id="9b68c-152">Testy hello účinku převzetí služeb při selhání pro konkrétní službu oddíl, a nechat hello jiných služeb neovlivní.</span><span class="sxs-lookup"><span data-stu-id="9b68c-152">It tests hello effect of failover on a specific service partition while leaving hello other services unaffected.</span></span> <span data-ttu-id="9b68c-153">Jakmile je nakonfigurován s informace o oddílu cílové hello a dalších parametrů, spouští se jako nástroj na straně klienta, který používá rozhraní API jazyka C# nebo prostředí PowerShell toogenerate chyb pro oddíl služby.</span><span class="sxs-lookup"><span data-stu-id="9b68c-153">Once it's configured with hello target partition information and other parameters, it runs as a client-side tool that uses either C# APIs or PowerShell toogenerate faults for a service partition.</span></span> <span data-ttu-id="9b68c-154">scénář Hello iteruje posloupnost simulované chyb a ověření služby průběhu obchodní logiky na straně tooprovide hello zatížení.</span><span class="sxs-lookup"><span data-stu-id="9b68c-154">hello scenario iterates through a sequence of simulated faults and service validation while your business logic runs on hello side tooprovide a workload.</span></span> <span data-ttu-id="9b68c-155">Chyby při ověřování služby označuje potíže, které potřebuje další šetření.</span><span class="sxs-lookup"><span data-stu-id="9b68c-155">A failure in service validation indicates an issue that needs further investigation.</span></span>

### <a name="faults-simulated-in-hello-failover-test"></a><span data-ttu-id="9b68c-156">Chyby simulated v testu převzetí služeb při selhání hello</span><span class="sxs-lookup"><span data-stu-id="9b68c-156">Faults simulated in hello failover test</span></span>
* <span data-ttu-id="9b68c-157">Restartujte nasazený balíček kódu je hostitelem oddílu hello</span><span class="sxs-lookup"><span data-stu-id="9b68c-157">Restart a deployed code package where hello partition is hosted</span></span>
* <span data-ttu-id="9b68c-158">Odebrat primární a sekundární repliky nebo bezstavové instance</span><span class="sxs-lookup"><span data-stu-id="9b68c-158">Remove a primary/secondary replica or stateless instance</span></span>
* <span data-ttu-id="9b68c-159">Restartujte primární sekundární repliky (Pokud trvalou služba)</span><span class="sxs-lookup"><span data-stu-id="9b68c-159">Restart a primary secondary replica (if a persisted service)</span></span>
* <span data-ttu-id="9b68c-160">Přesunutí primární repliky</span><span class="sxs-lookup"><span data-stu-id="9b68c-160">Move a primary replica</span></span>
* <span data-ttu-id="9b68c-161">Přesunutí sekundární repliky</span><span class="sxs-lookup"><span data-stu-id="9b68c-161">Move a secondary replica</span></span>
* <span data-ttu-id="9b68c-162">Restartujte hello oddílu</span><span class="sxs-lookup"><span data-stu-id="9b68c-162">Restart hello partition</span></span>

<span data-ttu-id="9b68c-163">testovací převzetí služeb při selhání Hello indukuje zvolené chybu a spustí ověření na hello služby tooensure jeho stability.</span><span class="sxs-lookup"><span data-stu-id="9b68c-163">hello failover test induces a chosen fault and then runs validation on hello service tooensure its stability.</span></span> <span data-ttu-id="9b68c-164">testovací převzetí služeb při selhání Hello indukuje jen jeden poruch na čas, názvem na rozdíl od toopossible několik chyb v testu chaos hello.</span><span class="sxs-lookup"><span data-stu-id="9b68c-164">hello failover test induces only one fault at a time, as opposed toopossible multiple faults in hello chaos test.</span></span> <span data-ttu-id="9b68c-165">Pokud hello služby oddílu není stabilizaci v rámci hello nakonfigurovaný časový limit po každé selhání, hello test se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="9b68c-165">If hello service partition does not stabilize within hello configured timeout after each fault, hello test fails.</span></span> <span data-ttu-id="9b68c-166">Hello test indukuje pouze bezpečné chyb.</span><span class="sxs-lookup"><span data-stu-id="9b68c-166">hello test induces only safe faults.</span></span> <span data-ttu-id="9b68c-167">To znamená, existovat externí selhání, nedojde ke ztrátě kvora nebo data.</span><span class="sxs-lookup"><span data-stu-id="9b68c-167">This means that in absence of external failures, a quorum or data loss will not occur.</span></span>

### <a name="important-configuration-options"></a><span data-ttu-id="9b68c-168">Důležité konfigurační možnosti</span><span class="sxs-lookup"><span data-stu-id="9b68c-168">Important configuration options</span></span>
* <span data-ttu-id="9b68c-169">**Partitionselector nejde**: výběr objektu, který určuje hello oddíl, který potřebuje toobe cílem.</span><span class="sxs-lookup"><span data-stu-id="9b68c-169">**PartitionSelector**: Selector object that specifies hello partition that needs toobe targeted.</span></span>
* <span data-ttu-id="9b68c-170">**TimeToRun**: celkový čas hello testu se spustí před dokončením instalace.</span><span class="sxs-lookup"><span data-stu-id="9b68c-170">**TimeToRun**: Total time that hello test will run before finishing.</span></span>
* <span data-ttu-id="9b68c-171">**MaxServiceStabilizationTimeout**: maximální množství času toowait pro toobecome clusteru hello před selháním hello testu v pořádku.</span><span class="sxs-lookup"><span data-stu-id="9b68c-171">**MaxServiceStabilizationTimeout**: Maximum amount of time toowait for hello cluster toobecome healthy before failing hello test.</span></span> <span data-ttu-id="9b68c-172">Hello provést kontroly jsou jestli stav služby je v pořádku, hello velikost cílové sady replik je dosaženo pro všechny oddíly, a neexistují žádné replik InBuild.</span><span class="sxs-lookup"><span data-stu-id="9b68c-172">hello checks performed are whether service health is OK, hello target replica set size is achieved for all partitions, and no InBuild replicas exist.</span></span>
* <span data-ttu-id="9b68c-173">**WaitTimeBetweenFaults**: množství času toowait mezi každý cyklus selhání a ověření.</span><span class="sxs-lookup"><span data-stu-id="9b68c-173">**WaitTimeBetweenFaults**: Amount of time toowait between every fault and validation cycle.</span></span>

### <a name="how-toorun-hello-failover-test"></a><span data-ttu-id="9b68c-174">Jak otestovat toorun hello převzetí služeb při selhání</span><span class="sxs-lookup"><span data-stu-id="9b68c-174">How toorun hello failover test</span></span>
<span data-ttu-id="9b68c-175">**C#**</span><span class="sxs-lookup"><span data-stu-id="9b68c-175">**C#**</span></span>

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

        // hello chaos test scenario should run at least 60 minutes or until it fails.
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

        // Create hello scenario class and execute it asynchronously.
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


<span data-ttu-id="9b68c-176">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="9b68c-176">**PowerShell**</span></span>

```powershell
$connection = "localhost:19000"
$timeToRun = 60
$maxStabilizationTimeSecs = 180
$waitTimeBetweenFaultsSec = 10
$serviceName = "fabric:/SampleApp/SampleService"

Connect-ServiceFabricCluster $connection

Invoke-ServiceFabricFailoverTestScenario -TimeToRunMinute $timeToRun -MaxServiceStabilizationTimeoutSec $maxStabilizationTimeSecs -WaitTimeBetweenFaultsSec $waitTimeBetweenFaultsSec -ServiceName $serviceName -PartitionKindSingleton
```
