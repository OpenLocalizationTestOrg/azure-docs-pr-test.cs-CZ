---
title: "selhání aaaSimulate v Azure mikroslužeb | Microsoft Docs"
description: "V tomto článku bude zmíněn hello testovatelnosti akce v Microsoft Azure Service Fabric nalezen."
services: service-fabric
documentationcenter: .net
author: motanv
manager: timlt
editor: toddabel
ms.assetid: ed53ca5c-4d5e-4b48-93c9-e386f32d8b7a
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/07/2017
ms.author: motanv;heeldin
ms.openlocfilehash: 5bdda1c0c5a40b243ab956c4791afd52e11c4089
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="testability-actions"></a>Testovatelnosti akce
V pořadí toosimulate nespolehlivé infrastruktury poskytuje Azure Service Fabric vás hello vývojáři a s způsoby toosimulate různé reálného selhání a přechodů mezi stavy. Tyto jsou zveřejněné jako testovatelnosti akce. Hello akce jsou hello nízké úrovně rozhraní API, která způsobí ověření, přechod stavu nebo konkrétní chyby vkládání. Kombinací těchto akcí můžete napsat scénáře komplexní testování pro vaše služby.

Service Fabric najdete že některé běžné scénáře, test se skládá z těchto akcí. Důrazně doporučujeme, aby využíváte těchto předdefinovaných scénáře, které jsou pečlivě zvolili tootest běžné přechodů mezi stavy a selhání případy. Akce však může být použité toocreate scénáře vlastní testování, pokud chcete tooadd pokrytí pro scénáře, která nespadají pod předdefinované scénáře hello ještě nebo které jsou vlastní, přizpůsobené pro vaši aplikaci.

C# implementace hello akce se nacházejí v hello System.Fabric.dll sestavení. modul prostředí PowerShell systému prostředků infrastruktury Hello v hello Microsoft.ServiceFabric.Powershell.dll sestavení nebyl nalezen. Jako součást instalace modulu runtime hello modul ServiceFabric PowerShell je nainstalovaný tooallow pro snadné použití.

## <a name="graceful-vs-ungraceful-fault-actions"></a>Řádné oproti vynuceném selhání akce
Akce testovatelnosti se zařazují do dvou hlavních kbelíků:

* Vynuceném chyb: Simulace selhání, jako je restartování počítače tyto chyby a zpracování dojde k chybě. V takových případech chyb zastaví náhle hello kontext spuštění procesu. To znamená, že žádné čištění hello stavu mohou spouštět před spuštěním aplikace hello znovu.
* Řádné chyb: tyto chyby simulace řádně akcí, jako jsou repliky přesune a vyřazuje aktivovány Vyrovnávání zatížení. V takových případech hello služba získá upozornění hello zavřít a můžete vyčistit hello stavu před ukončením.

Pro lepší kvalitu ověření spuštění služby hello a obchodní zatížení při vyvolat různé řádně a vynuceném chyb. Při vynuceném chyb výkonu scénáře, kde hello služby neočekávaného ukončení procesu uprostřed hello některé pracovního postupu. Tato testy hello obnovení cesta ihned po hello služby repliky pomocí Service Fabric. To vám pomůže testování konzistenci dat a zda je stav služby hello udržovat správně po selhání. Hello další sadu chyb (hello řádně selhání) otestovat jestli reakce hello služby správně tooreplicas přenášeny pomocí Service Fabric. Toto zpracování zrušení testů v metodě RunAsync hello. Služba Hello musí toocheck pro hello zrušení tokenu se nastavit správně uložit stav a ukončete metodě RunAsync hello.

## <a name="testability-actions-list"></a>Seznam akcí testovatelnosti
| Akce | Popis | Spravovaná rozhraní API | Rutiny prostředí PowerShell | Řádné/vynuceném chyb |
| --- | --- | --- | --- | --- |
| CleanTestState |Odebere všechny stavy testovací hello hello clusteru v případě chybný vypnutí hello testovací ovladače. |CleanTestStateAsync |Remove-ServiceFabricTestState |Neuvedeno |
| InvokeDataLoss |Indukuje ztráty dat do oddílu služby. |InvokeDataLossAsync |Invoke-ServiceFabricPartitionDataLoss |Řádné |
| InvokeQuorumLoss |Oddíl dané stavové služby umístí do ztrátě kvora. |InvokeQuorumLossAsync |Vyvolání ServiceFabricQuorumLoss |Řádné |
| Přesunutí primární |Přesune hello zadat primární repliky stavové služby toohello zadaný uzel clusteru. |MovePrimaryAsync |Přesunutí ServiceFabricPrimaryReplica |Řádné |
| Přesunout sekundární |Přesune hello aktuální sekundární repliky do stavové služby tooa jiného uzlu clusteru. |MoveSecondaryAsync |Přesunutí ServiceFabricSecondaryReplica |Řádné |
| RemoveReplica |Repliky selhání simuluje odebráním repliku z clusteru. To se zavře hello repliky a přejde se toorole 'None', všechny její stav odebrání z clusteru hello. |RemoveReplicaAsync |Odebrat ServiceFabricReplica |Řádné |
| RestartDeployedCodePackage |Simuluje selhání procesu balíček kódu restartováním balíček kódu nasazené na uzlu v clusteru. To zruší hello kód balíček procesu, který restartuje všechny hello uživatele služby repliky hostované v tomto procesu. |RestartDeployedCodePackageAsync |Restartování ServiceFabricDeployedCodePackage |Vynuceném |
| RestartNode |Selhání uzlu clusteru Service Fabric simuluje restartováním uzlu. |RestartNodeAsync |Restartování ServiceFabricNode |Vynuceném |
| RestartPartition |Simuluje scénáři přerušení spojení přerušení spojení nebo cluster datacentra restartováním některé nebo všechny repliky oddílu. |RestartPartitionAsync |Restart-ServiceFabricPartition |Řádné |
| RestartReplica |Repliky selhání simuluje restartování trvalou repliky v clusteru, zavřením hello repliky a pak ho znovu. |RestartReplicaAsync |Restartování ServiceFabricReplica |Řádné |
| Počáteční_uzel |Spustí uzlu v clusteru, který je již zastaveno. |StartNodeAsync |Start-ServiceFabricNode |Neuvedeno |
| Příkaz StopNode |Selhání uzlu simuluje podle zastavení uzlu v clusteru. Hello uzlu zůstanou dolů, dokud se nazývá Počáteční_uzel. |StopNodeAsync |Stop-ServiceFabricNode |Vynuceném |
| ValidateApplication |Ověří dostupnost hello a stav všech služeb Service Fabric v rámci aplikace, obvykle po vyvolat některé chyby do systému hello. |ValidateApplicationAsync |Test ServiceFabricApplication |Neuvedeno |
| ValidateService |Ověří dostupnost hello a stav služby Service Fabric, obvykle po vyvolat některé chyby do systému hello. |ValidateServiceAsync |Test ServiceFabricService |Neuvedeno |

## <a name="running-a-testability-action-using-powershell"></a>Spuštění testovatelnosti akci pomocí prostředí PowerShell
Tento kurz ukazuje, jak toorun testovatelnosti akce pomocí prostředí PowerShell. Se dozvíte, jak toorun testovatelnosti akce proti místní cluster (jedna box) nebo Azure clusteru. Microsoft.Fabric.Powershell.dll – hello modul Service Fabric prostředí PowerShell – se nainstaluje automaticky při instalaci hello Microsoft Service Fabric MSI. modul Hello je načten automaticky při otevření příkazovém řádku prostředí PowerShell.

Kurz segmenty:

* [Spouštění akce clusteru s podporou jeden pole](#run-an-action-against-a-one-box-cluster)
* [Spouštění akce clusteru služby Azure](#run-an-action-against-an-azure-cluster)

### <a name="run-an-action-against-a-one-box-cluster"></a>Spouštění akce clusteru s podporou jeden pole
toorun testovatelnosti opatření proti místní cluster, nejdřív připojit toohello cluster a otevřete hello příkazovém řádku prostředí PowerShell v režimu správce. Dejte nám podívejte se na hello **restartování ServiceFabricNode** akce.

```powershell
Restart-ServiceFabricNode -NodeName Node1 -CompletionMode DoNotVerify
```

Zde hello akce **restartování ServiceFabricNode** je spuštěn na uzlu s názvem "Uzel1". režim dokončení Hello Určuje, by neměl ověřit, zda text hello restartování uzlu akce ve skutečnosti úspěšně. Zadání režim dokončení hello jako "Ověřit" způsobí, že tooverify jestli akce restartování hello ve skutečnosti bylo úspěšné. Namísto uzlu hello přímo zadat jeho název, můžete zadat pomocí druh oddílu klíč a hello repliky, následujícím způsobem:

```powershell
Restart-ServiceFabricNode -ReplicaKindPrimary  -PartitionKindNamed -PartitionKey Partition3 -CompletionMode Verify
```


```powershell
$connection = "localhost:19000"
$nodeName = "Node1"

Connect-ServiceFabricCluster $connection
Restart-ServiceFabricNode -NodeName $nodeName -CompletionMode DoNotVerify
```

**Restartování ServiceFabricNode** by měla být použité toorestart Service Fabric uzel v clusteru. To se zastaví hello Fabric.exe procesu, který restartuje všechny hello systému služby a uživatele služby repliky hostované v tomto uzlu. Pomocí této tootest rozhraní API služby pomáhá odkrýt chyby podél hello převzetí služeb při selhání obnovení cesty. Pomáhá simulace selhání uzlu v clusteru hello.

Hello následující snímek obrazovky ukazuje hello **restartování ServiceFabricNode** testovatelnosti příkaz v akci.

![](media/service-fabric-testability-actions/Restart-ServiceFabricNode.png)

Hello výstup hello nejprve **Get-ServiceFabricNode** (použití rutiny z modulu Service Fabric prostředí PowerShell hello) zobrazuje, že hello místní cluster s pěti uzly: Node.1 tooNode.5. Po akci testovatelnosti hello (rutiny) **restartování ServiceFabricNode** se spouští na hello uzel s názvem Node.4, vidíme, tento uzel hello provozu byl obnoven.

### <a name="run-an-action-against-an-azure-cluster"></a>Spouštění akce clusteru služby Azure
Spuštění akci testovatelnosti (pomocí prostředí PowerShell) proti clusteru služby Azure je podobné akce hello toorunning proti místní cluster. Hello jen rozdílem je, že před spuštěním akce hello místo připojování toohello místní cluster, je nutné tooconnect toohello Azure nejprve clusteru.

## <a name="running-a-testability-action-using-c35"></a>Spuštění testovatelnosti akci pomocí C &#35;
toorun testovatelnosti akce pomocí jazyka C#, je třeba nejprve tooconnect toohello clusteru pomocí FabricClient. Poté získejte hello potřebné parametry toorun hello akce. Můžete použít jiné parametry toorun hello stejnou akci.
Prohlížení hello RestartServiceFabricNode akce, jedním ze způsobů toorun, je pomocí hello uzlu informace (název uzlu a ID instance uzlu) v clusteru hello.

```csharp
RestartNodeAsync(nodeName, nodeInstanceId, completeMode, operationTimeout, CancellationToken.None)
```

Vysvětlení parametr:

* **CompleteMode** Určuje, že hello režimu by neměl ověřit, zda akce restartování hello ve skutečnosti úspěšně. Zadání režim dokončení hello jako "Ověřit" způsobí, že tooverify jestli akce restartování hello ve skutečnosti bylo úspěšné.  
* **OperationTimeout** nastaví hello doba hello operaci toofinish předtím, než je vyvolána výjimka TimeoutException.
* **CancellationToken** umožňuje čekající volání toobe, došlo ke zrušení.

Namísto uzlu hello přímo zadat jeho název, můžete zadat pomocí druh oddílu klíč a hello repliky.

Další informace najdete v tématu [partitionselector nejde a replicaselector nejde](#partition_replica_selector).

```csharp
// Add a reference tooSystem.Fabric.Testability.dll and System.Fabric.dll
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Fabric.Testability;
using System.Fabric;
using System.Threading;
using System.Numerics;

class Test
{
    public static int Main(string[] args)
    {
        string clusterConnection = "localhost:19000";
        Uri serviceName = new Uri("fabric:/samples/PersistentToDoListApp/PersistentToDoListService");
        string nodeName = "N0040";
        BigInteger nodeInstanceId = 130743013389060139;

        Console.WriteLine("Starting RestartNode test");
        try
        {
            //Restart hello node by using ReplicaSelector
            RestartNodeAsync(clusterConnection, serviceName).Wait();

            //Another way toorestart node is by using nodeName and nodeInstanceId
            RestartNodeAsync(clusterConnection, nodeName, nodeInstanceId).Wait();
        }
        catch (AggregateException exAgg)
        {
            Console.WriteLine("RestartNode did not complete: ");
            foreach (Exception ex in exAgg.InnerExceptions)
            {
                if (ex is FabricException)
                {
                    Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
                }
            }
            return -1;
        }

        Console.WriteLine("RestartNode completed.");
        return 0;
    }

    static async Task RestartNodeAsync(string clusterConnection, Uri serviceName)
    {
        PartitionSelector randomPartitionSelector = PartitionSelector.RandomOf(serviceName);
        ReplicaSelector primaryofReplicaSelector = ReplicaSelector.PrimaryOf(randomPartitionSelector);

        // Create FabricClient with connection and security information here
        FabricClient fabricclient = new FabricClient(clusterConnection);
        await fabricclient.FaultManager.RestartNodeAsync(primaryofReplicaSelector, CompletionMode.Verify);
    }

    static async Task RestartNodeAsync(string clusterConnection, string nodeName, BigInteger nodeInstanceId)
    {
        // Create FabricClient with connection and security information here
        FabricClient fabricclient = new FabricClient(clusterConnection);
        await fabricclient.FaultManager.RestartNodeAsync(nodeName, nodeInstanceId, CompletionMode.Verify);
    }
}
```

## <a name="partitionselector-and-replicaselector"></a>Partitionselector nejde a replicaselector nejde
### <a name="partitionselector"></a>Partitionselector nejde
Partitionselector nejde je pomocné rutiny v testovatelnosti a je použité tooselect konkrétní rozdělit na oddíly na které tooperform jednu z akcí testovatelnosti hello. Pokud je ID oddílu hello znám předem, může být použité tooselect na konkrétní oddíl. Můžete zadat klíč oddílu hello a hello operace bude vyřešen ID oddílu hello interně. Máte také možnost hello výběru náhodného oddílu.

toouse tohoto pomocníka vytvořit objekt partitionselector nejde hello a vyberte oddíl hello pomocí jedné z hello vyberte * metody. Pak předejte v hello partitionselector nejde objekt toohello rozhraní API, které vyžaduje. Pokud je vybraná žádná možnost, bude výchozí tooa náhodných oddílu.

```csharp
Uri serviceName = new Uri("fabric:/samples/InMemoryToDoListApp/InMemoryToDoListService");
Guid partitionIdGuid = new Guid("8fb7ebcc-56ee-4862-9cc0-7c6421e68829");
string partitionName = "Partition1";
Int64 partitionKeyUniformInt64 = 1;

// Select a random partition
PartitionSelector randomPartitionSelector = PartitionSelector.RandomOf(serviceName);

// Select a partition based on ID
PartitionSelector partitionSelectorById = PartitionSelector.PartitionIdOf(serviceName, partitionIdGuid);

// Select a partition based on name
PartitionSelector namedPartitionSelector = PartitionSelector.PartitionKeyOf(serviceName, partitionName);

// Select a partition based on partition key
PartitionSelector uniformIntPartitionSelector = PartitionSelector.PartitionKeyOf(serviceName, partitionKeyUniformInt64);
```

### <a name="replicaselector"></a>Replicaselector nejde
Replicaselector nejde je pomocné rutiny v testovatelnosti a je použité toohelp vyberte repliku, na které tooperform některé hello testovatelnosti akcí. Pokud je ID repliky hello znám předem, může být použité tooselect konkrétní repliky. Kromě toho máte možnost hello výběru primární repliku nebo náhodné sekundární lokality. Replicaselector nejde je odvozena z partitionselector nejde, proto musíte tooselect obě hello repliky a hello oddílu, na kterém chcete tooperform hello testovatelnosti operaci.

toouse tohoto pomocníka vytvořit objekt replicaselector nejde a nastavte požadovaným způsobem hello tooselect hello repliky a hello oddílu. Pak předejte ji do hello rozhraní API, které vyžaduje. Pokud je vybraná žádná možnost, bude výchozí tooa náhodných repliky a náhodných oddílu.

```csharp
Guid partitionIdGuid = new Guid("8fb7ebcc-56ee-4862-9cc0-7c6421e68829");
PartitionSelector partitionSelector = PartitionSelector.PartitionIdOf(serviceName, partitionIdGuid);
long replicaId = 130559876481875498;

// Select a random replica
ReplicaSelector randomReplicaSelector = ReplicaSelector.RandomOf(partitionSelector);

// Select hello primary replica
ReplicaSelector primaryReplicaSelector = ReplicaSelector.PrimaryOf(partitionSelector);

// Select hello replica by ID
ReplicaSelector replicaByIdSelector = ReplicaSelector.ReplicaIdOf(partitionSelector, replicaId);

// Select a random secondary replica
ReplicaSelector secondaryReplicaSelector = ReplicaSelector.RandomSecondaryOf(partitionSelector);
```

## <a name="next-steps"></a>Další kroky
* [Testovatelnosti scénáře](service-fabric-testability-scenarios.md)
* Jak tootest služby
  * [Simulace selhání během úloh služeb](service-fabric-testability-workload-tests.md)
  * [Selhání komunikace Service-to-service](service-fabric-testability-scenarios-service-communication.md)

