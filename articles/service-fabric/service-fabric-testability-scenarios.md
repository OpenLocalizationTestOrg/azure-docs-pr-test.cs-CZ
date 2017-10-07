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
# <a name="testability-scenarios"></a>Testovatelnosti scénáře
Velkých distribuovaných systémech stejně, jako jsou ze své podstaty nespolehlivé cloudových infrastruktur. Azure Service Fabric nabízí vývojářům hello možnost toowrite služby toorun nad nespolehlivé infrastruktury. V pořadí toowrite vysoce kvalitních služeb vývojáři nutné mít tooinduce toobe takové nespolehlivé infrastruktury tootest hello stabilitu své služby.

Hello selhání Analysis Service poskytuje vývojářům hello možnost tooinduce selhání akce tootest služby v hello přítomnost chyb. Ale cílové simulované chyb získáte pouze, pokud. testování navíc hello tootake hello testovací scénáře můžete použít v Service Fabric: testovací chaos a testovací převzetí služeb při selhání. Tyto scénáře simulovat průběžné prokládaná chyb, řádně a vynuceném v rámci clusteru hello přes dlouhou dobu. Jakmile testu je nakonfigurovaný s hello rychlost a druh chyb, může být spuštěn prostřednictvím rozhraní API jazyka C# nebo prostředí PowerShell, toogenerate chyb v hello clusteru a služby.

> [!WARNING]
> ChaosTestScenario je nahrazován Chaos pružnější, na základě služeb. Naleznete v článku nové toohello [řídí Chaos](service-fabric-controlled-chaos.md) další podrobnosti.
> 
> 

## <a name="chaos-test"></a>Chaos testu
scénář chaos Hello generuje chyby napříč hello celý cluster Service Fabric. scénář Hello komprimaci chyb obecně zobrazená v měsíců nebo let tooa několik hodin. kombinace Hello prokládaná chyb hello vysokou odolnost míru vyhledá náročnějších případech, které jsou jinak vynechat. To vede tooa výrazné zlepšení kvality kódu hello hello služby.

### <a name="faults-simulated-in-hello-chaos-test"></a>Chyby simulated v testu chaos hello
* Restartovat uzel
* Restartujte nasazený balíček kódu
* Odstranění repliky
* Restartujte repliky
* Přesunutí primární repliky (volitelné)
* Přesunutí sekundární repliky (volitelné)

Hello chaos testovací běhy více opakování chyb a ověření clusteru pro hello zadané časové období. Hello času stráveného pro toostabilize hello clusteru a pro ověření toosucceed je také možné konfigurovat. scénář Hello selže, když dosáhl jediné chyby v ověření clusteru.

Zvažte například, že testu toorun nastaven na jednu hodinu s maximálně tři souběžných chyb. Hello test se vyvolat tři chyb a pak ověřte stav clusteru hello. Hello test bude iteraci v rámci předchozí krok hello dokud hello clusteru se změní na není v pořádku nebo předá jednu hodinu. Pokud hello cluster se změní na není v pořádku v kteroukoli iteraci, tj. není stabilizaci v nakonfigurovaném čase, hello test se nezdaří s výjimkou. Tato výjimka označuje, že něco nepovede a potřebuje další šetření.

V současné podobě indukuje hello selhání generování modul v testu chaos hello pouze bezpečné chyb. To znamená, že v hello chybí externí chyb, ke ztrátě kvora nebo data nikdy nedojde.

### <a name="important-configuration-options"></a>Důležité konfigurační možnosti
* **TimeToRun**: celkový čas testu hello bude spuštěn před dokončením s úspěch. Hello test můžete dokončit dříve místo selhání ověření.
* **MaxClusterStabilizationTimeout**: maximální množství času toowait pro toobecome clusteru hello před selháním hello testu v pořádku. Hello provést kontroly jsou jestli stav clusteru je v pořádku, stav služby je v pořádku, hello velikost cílové sady replik se dá dosáhnout hello oddílu služby a neexistují žádné replik InBuild.
* **MaxConcurrentFaults**: maximální počet souběžných chyb vyvolané v každé iteraci. Dobrý den vyšší číslo hello, hello agresivnější hello test, proto výsledkem složitější převzetí služeb při selhání a kombinace přechodu. Hello test zaručuje, že neexistence externí chyb nebude existovat kvora nebo ztráty dat, bez ohledu na to, jak vysoké tato konfigurace je.
* **EnableMoveReplicaFaults**: Povolí nebo zakáže hello chyb, které způsobují hello přesun hello primární nebo sekundární repliky. Tyto chyby jsou ve výchozím nastavení zakázány.
* **WaitTimeBetweenIterations**: množství času toowait mezi iterací, např. po zaokrouhlit chyb a odpovídající ověření.

### <a name="how-toorun-hello-chaos-test"></a>Jak toorun hello chaos testování
Ukázka v jazyce C#

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

PowerShell

```powershell
$connection = "localhost:19000"
$timeToRun = 60
$maxStabilizationTimeSecs = 180
$concurrentFaults = 3
$waitTimeBetweenIterationsSec = 60

Connect-ServiceFabricCluster $connection

Invoke-ServiceFabricChaosTestScenario -TimeToRunMinute $timeToRun -MaxClusterStabilizationTimeoutSec $maxStabilizationTimeSecs -MaxConcurrentFaults $concurrentFaults -EnableMoveReplicaFaults -WaitTimeBetweenIterationsSec $waitTimeBetweenIterationsSec
```


## <a name="failover-test"></a>Testovací převzetí služeb při selhání
scénáře testovacího převzetí služeb při selhání Hello je verze hello chaos testovací scénář, který cílí na konkrétní službu oddíl. Testy hello účinku převzetí služeb při selhání pro konkrétní službu oddíl, a nechat hello jiných služeb neovlivní. Jakmile je nakonfigurován s informace o oddílu cílové hello a dalších parametrů, spouští se jako nástroj na straně klienta, který používá rozhraní API jazyka C# nebo prostředí PowerShell toogenerate chyb pro oddíl služby. scénář Hello iteruje posloupnost simulované chyb a ověření služby průběhu obchodní logiky na straně tooprovide hello zatížení. Chyby při ověřování služby označuje potíže, které potřebuje další šetření.

### <a name="faults-simulated-in-hello-failover-test"></a>Chyby simulated v testu převzetí služeb při selhání hello
* Restartujte nasazený balíček kódu je hostitelem oddílu hello
* Odebrat primární a sekundární repliky nebo bezstavové instance
* Restartujte primární sekundární repliky (Pokud trvalou služba)
* Přesunutí primární repliky
* Přesunutí sekundární repliky
* Restartujte hello oddílu

testovací převzetí služeb při selhání Hello indukuje zvolené chybu a spustí ověření na hello služby tooensure jeho stability. testovací převzetí služeb při selhání Hello indukuje jen jeden poruch na čas, názvem na rozdíl od toopossible několik chyb v testu chaos hello. Pokud hello služby oddílu není stabilizaci v rámci hello nakonfigurovaný časový limit po každé selhání, hello test se nezdaří. Hello test indukuje pouze bezpečné chyb. To znamená, existovat externí selhání, nedojde ke ztrátě kvora nebo data.

### <a name="important-configuration-options"></a>Důležité konfigurační možnosti
* **Partitionselector nejde**: výběr objektu, který určuje hello oddíl, který potřebuje toobe cílem.
* **TimeToRun**: celkový čas hello testu se spustí před dokončením instalace.
* **MaxServiceStabilizationTimeout**: maximální množství času toowait pro toobecome clusteru hello před selháním hello testu v pořádku. Hello provést kontroly jsou jestli stav služby je v pořádku, hello velikost cílové sady replik je dosaženo pro všechny oddíly, a neexistují žádné replik InBuild.
* **WaitTimeBetweenFaults**: množství času toowait mezi každý cyklus selhání a ověření.

### <a name="how-toorun-hello-failover-test"></a>Jak otestovat toorun hello převzetí služeb při selhání
**C#**

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


**PowerShell**

```powershell
$connection = "localhost:19000"
$timeToRun = 60
$maxStabilizationTimeSecs = 180
$waitTimeBetweenFaultsSec = 10
$serviceName = "fabric:/SampleApp/SampleService"

Connect-ServiceFabricCluster $connection

Invoke-ServiceFabricFailoverTestScenario -TimeToRunMinute $timeToRun -MaxServiceStabilizationTimeoutSec $maxStabilizationTimeSecs -WaitTimeBetweenFaultsSec $waitTimeBetweenFaultsSec -ServiceName $serviceName -PartitionKindSingleton
```
