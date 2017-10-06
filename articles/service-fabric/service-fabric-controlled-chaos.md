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
# <a name="induce-controlled-chaos-in-service-fabric-clusters"></a>Vyvolat řízené Chaos v prostředí clusterů Service Fabric
Ve velkém měřítku distribuovaných systémů, jako jsou ze své podstaty nespolehlivé cloudových infrastruktur. Azure Service Fabric umožňuje vývojářům toowrite spolehlivé distribuované služby nespolehlivé infrastruktuře. toowrite robustní distribuované služby nespolehlivé infrastruktuře, vývojáři třeba toobe možné tootest hello stabilitu své služby při hello základní nespolehlivé infrastruktury prochází přes přechodů mezi stavy složité kvůli toofaults.

Hello [vkládání selhání a clusteru Analysis Service](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-testability-overview) (také označované jako hello selhání Analysis Service) poskytuje vývojářům možnost hello tooinduce chyb tootest své služby. Tyto cílové simulated chyb, jako je třeba [restartování oddíl](https://docs.microsoft.com/en-us/powershell/module/servicefabric/start-servicefabricpartitionrestart?view=azureservicefabricps), může pomoci prověření hello nejběžnější přechodů mezi stavy. Ale cílové simulované chyb jsou tendenční podle definice a proto může dojít chyby které zobrazují se pouze v pevném předpovědi, dlouhé a komplikované posloupnost přechodů mezi stavy. Pro neposunutého testování, můžete použít Chaos.

Chaos simuluje pravidelné, prokládaná chyb (řádně i vynuceném) v rámci clusteru hello přes dlouhou dobu. Po konfiguraci Chaos s hello rychlost a hello druh chyb, můžete začít Chaos prostřednictvím jazyka C# nebo rozhraní API prostředí Powershell toostart generování chyb v clusteru hello a vaše služby. Můžete nakonfigurovat Chaos toorun pro zadané časové období (například pro jednu hodinu), po jejímž uplynutí Chaos zastaví automaticky, nebo můžete volat rozhraní API StopChaos (C# nebo prostředí Powershell) toostop ho kdykoli.

> [!NOTE]
> V současné podobě Chaos indukuje pouze bezpečné chyb, což znamená, že hello neexistence externí chyb ztrátě kvora nebo ztrátě dat se nikdy neprovádí.
>

Když Chaos je spuštěný, vytváří různé události, které zaznamenat stav hello hello spustit momentálně hello. Například ExecutingFaultsEvent obsahuje všechny hello chyb, Chaos se rozhodla tooexecute v této iteraci. ValidationFailedEvent obsahuje hello podrobné informace o selhání ověření (stavu nebo stabilitu problémy), který byl nalezen během ověřování hello hello clusteru. Můžete vyvolat hello GetChaosReport API (C# nebo prostředí Powershell) tooget hello sestavu Chaos spustí. Tyto události získat uchovávané v [spolehlivé slovník](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-reliable-services-reliable-collections), který má zásady zkrácení závisí na dvě konfigurace: **MaxStoredChaosEventCount** (výchozí hodnota je 25 000) a **StoredActionCleanupIntervalInSeconds** (výchozí hodnota je 3600). Každý *StoredActionCleanupIntervalInSeconds* Chaos kontroly a všechny ale hello nejnovější *MaxStoredChaosEventCount* události, jsou vymazány ze slovníku spolehlivé hello.

## <a name="faults-induced-in-chaos"></a>Vyvolané v Chaos chyb
Chaos generuje chyby napříč hello celý cluster Service Fabric a komprimaci chyb, které jsou zobrazená v měsíců nebo let do několik hodin. kombinace Hello prokládaná chyb hello vysokou odolnost míru vyhledá náročnějších případech, které jinak může být načteni. Toto cvičení Chaos vede tooa výrazné zlepšení kvality kódu hello hello služby.

Chaos indukuje chyb z hello následujících kategorií:

* Restartovat uzel
* Restartujte nasazený balíček kódu
* Odstranění repliky
* Restartujte repliky
* Přesunutí primární repliky (Konfigurovat)
* Přesunutí sekundární repliky (Konfigurovat)

Chaos běží ve více iterací. Každé iteraci se skládá z chyb a ověření clusteru pro hello zadané období. Můžete nakonfigurovat hello času stráveného pro hello clusteru toostabilize a toosucceed ověření. Pokud selhání naleznete v ověření clusteru, Chaos generuje a přetrvává ValidationFailedEvent s časovým razítkem UTC hello a podrobnosti o chybě hello. Představte si třeba instanci Chaos, který je nastavený toorun jednu hodinu s maximálně tři souběžných chyb. Chaos indukuje tři chyb a poté ověří hello clusteru stavu. Prostřednictvím hello předchozí opakuje, předá krok, dokud není explicitně zastaven prostřednictvím hello StopChaosAsync API nebo jednu hodinu. Pokud hello cluster se změní na není v pořádku v kteroukoli iteraci (to znamená, že ho není stabilizovat v rámci hello předané MaxClusterStabilizationTimeout), Chaos generuje ValidationFailedEvent. Tato událost označuje, že něco nepovede a může být třeba další šetření.

tooget, který závady Chaos vyvolané, můžete použít rozhraní API GetChaosReport (prostředí powershell nebo C#). Hello API získá další segment hello hello Chaos sestavy na základě token pokračování předané hello nebo hello předané čas range. Můžete zadat hello ContinuationToken tooget hello další segment hello Chaos sestavy nebo můžete zadat časový rozsah hello prostřednictvím StartTimeUtc a EndTimeUtc, nemůžete ale zadat hello ContinuationToken i hello období v hello stejné volání. Když nejsou k dispozici více než 100 Chaos události, hello Chaos sestavy se vrátí v segmentech, kde segment obsahuje více než 100 Chaos událostí.

## <a name="important-configuration-options"></a>Důležité konfigurační možnosti
* **TimeToRun**: celkový čas která Chaos spouští před dokončením s úspěch. Můžete zastavit Chaos předtím, než byl spuštěn po dobu TimeToRun hello prostřednictvím hello StopChaos rozhraní API.

* **MaxClusterStabilizationTimeout**: hello maximální množství času toowait pro toobecome clusteru hello před generovala ValidationFailedEvent v pořádku. Toto čekání je tooreduce hello zatížení v clusteru hello, zatímco probíhá obnovení. provést kontroly Hello jsou:
  * Pokud stav clusteru hello je v pořádku
  * Pokud stav služby hello je v pořádku.
  * Pokud cílový repliky hello nastavit velikost se dá dosáhnout oddílu služby hello
  * Že neexistuje žádná replik InBuild
* **MaxConcurrentFaults**: maximální počet souběžných chyb, které jsou v každé iteraci vyvolané hello. vyšší číslo hello Hello, hello agresivnější je Chaos a prochází hello převzetí služeb při selhání a hello stavu přechodu kombinace, které hello clusteru jsou i složitější. 

> [!NOTE]
> Bez ohledu na to jak velkou hodnotu *MaxConcurrentFaults* má Chaos zaručuje - hello neexistence externí chyb - neexistuje ztrátě dat nebo ztrátě kvora.
>

* **EnableMoveReplicaFaults**: Povolí nebo zakáže hello chyb, které způsobují toomove hello primární nebo sekundární repliky. Tyto chyby jsou ve výchozím nastavení zakázány.
* **WaitTimeBetweenIterations**: hello množství času toowait mezi iterací. To znamená hello množství času, které Chaos se pozastaví po provedení zaokrouhlit chyb a nutnosti dokončení hello odpovídající ověření stavu hello hello clusteru. vyšší hodnota hello Hello hello nižší je míra vkládání průměrná selhání hello.
* **WaitTimeBetweenFaults**: hello množství času toowait mezi dvě po sobě jdoucích chyb v jednom iterace. Hello vyšší hodnota hello, hello nižší shodu hello (nebo hello překryv mezi) chyb.
* **ClusterHealthPolicy**: zásady stavu clusteru je použité toovalidate hello stavu hello clusteru mezi Chaos iterací. Pokud hello stav clusteru došlo k chybě nebo pokud se stane neočekávané výjimce během zpracování chyby, budou Chaos Počkejte 30 minut před hello další – kontrola stavu - tooprovide hello cluster s některé toorecuperate čas.
* **Kontext**: kolekce (řetězec, řetězec) zadejte páry klíč hodnota. Mapa Hello může být použité toorecord informace o hello Chaos spustit. Nemůže být více než 100 tyto dvojice a každý řetězec (klíč nebo hodnota) může mít maximálně povolených 4095 znaků dlouhé. Tato mapa je nastavena podle hello starter hello Chaos spustit toooptionally úložiště hello kontextu o hello konkrétní spustit.

## <a name="how-toorun-chaos"></a>Jak toorun Chaos

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
