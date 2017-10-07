---
title: "úlohy toorun závislosti úkolů aaaUse podle hello dokončení jiné úlohy – Azure Batch | Microsoft Docs"
description: "Vytvoření úlohy, které jsou závislé na dokončení jiné úlohy pro zpracování prostředí MapReduce style a podobné velkých objemů dat hello úlohy v Azure Batch."
services: batch
documentationcenter: .net
author: tamram
manager: timlt
editor: 
ms.assetid: b8d12db5-ca30-4c7d-993a-a05af9257210
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 05/22/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: faf08ec38cb30b1f66acd51e256c31aea6215c62
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-task-dependencies-toorun-tasks-that-depend-on-other-tasks"></a>Vytvoření závislosti úkolů toorun úlohy, které závisí na jiné úlohy

Toorun závislosti úkolů můžete definovat úlohu nebo sadu úloh, až po dokončení úlohy nadřazené. Některé scénáře, kde jsou užitečné závislosti úkolů patří:

* Úlohy MapReduce-style v cloudu hello.
* Úlohy, jejichž úloh zpracování dat může být vyjádřený jako necyklicky (DAG).
* Vykreslování před a po vykreslení procesy, kde každý úkol musí dokončit před zahájením hello další úlohy.
* Žádné jiné úlohy, ve kterém podřízené úlohy závisí na výstup hello nadřazeného úloh.

Pomocí závislosti úkolů dávky můžete vytvořit úlohy, které jsou naplánovány pro spuštění na výpočetních uzlech po dokončení jedné nebo více úloh nadřazené hello. Můžete například vytvořit úlohu, která vykreslí každý snímek 3D film samostatný, paralelní úlohy. Poslední úloha Hello – hello "sloučení úkolů"--sloučení hello vykresluje rámce do hello jenom dokončení film po všech rámců byl úspěšně vykreslen.

Ve výchozím nastavení závislé úlohy jsou naplánovány pro spuštění až po úspěšném dokončení úlohy nadřazené hello. Můžete zadat výchozí chování závislostí akce toooverride hello a spouštět úlohy, pokud úloha nadřazené hello selže. V tématu hello [závislostí akcí](#dependency-actions) podrobnosti.  

Můžete vytvořit úlohy, které jsou závislé na dalších úloh v relaci 1: 1 nebo 1 n. Můžete také vytvořit rozsah závislostí, kde úkol závisí na dokončení hello skupiny úloh v rámci zadaného rozsahu ID úkolu. Tyto tři relace m: n toocreate základní scénáře můžete kombinovat.

## <a name="task-dependencies-with-batch-net"></a>Závislosti úkolů pomocí rozhraní Batch .NET
V tomto článku probereme, jak závislosti úkolů tooconfigure pomocí hello [Batch .NET] [ net_msdn] knihovny. Nám nejdřív ukazují, jak příliš[povolit závislosti úkolů](#enable-task-dependencies) na úlohách a pak ukazují, jak příliš[úlohu nakonfigurovat závislosti](#create-dependent-tasks). Také popisují, jak toospecify závislostí akce toorun závislé úlohy Pokud hello nadřazené selže. Nakonec probereme hello [scénáře závislostí](#dependency-scenarios) podporující Batch.

## <a name="enable-task-dependencies"></a>Povolit závislosti úkolů
závislosti úkolů toouse ve vaší aplikaci Batch, musíte nejdřív nakonfigurovat závislosti úkolů toouse hello úlohy. V Batch .NET, povolte ji na vaše [CloudJob] [ net_cloudjob] nastavením jeho [UsesTaskDependencies] [ net_usestaskdependencies] vlastnost příliš`true`:

```csharp
CloudJob unboundJob = batchClient.JobOperations.CreateJob( "job001",
    new PoolInformation { PoolId = "pool001" });

// IMPORTANT: This is REQUIRED for using task dependencies.
unboundJob.UsesTaskDependencies = true;
```

V předchozím fragmentu kódu hello, "batchClient" představuje instanci hello [BatchClient] [ net_batchclient] třídy.

## <a name="create-dependent-tasks"></a>Vytvoření závislé úlohy
toocreate úlohu, která závisí na hello dokončení jedné nebo více úloh nadřazené, můžete zadat, která hello úkolů "závisí na" hello další úlohy. V Batch .NET, nakonfigurujte hello [CloudTask][net_cloudtask].[ DependsOn] [ net_dependson] vlastnost s instancí hello [TaskDependencies] [ net_taskdependencies] třídy:

```csharp
// Task 'Flowers' depends on completion of both 'Rain' and 'Sun'
// before it is run.
new CloudTask("Flowers", "cmd.exe /c echo Flowers")
{
    DependsOn = TaskDependencies.OnIds("Rain", "Sun")
},
```

Tento fragment kódu vytvoří závislé úlohy s ID úlohy "Květy". Hello "Květy" úkol závisí na úkoly "Deště" a "Sun". Úloha "Květy" bude až po úlohy, které byly úspěšně dokončeny "Deště" a "Sun" naplánované toorun na výpočetním uzlu.

> [!NOTE]
> Úloha je považován za toobe úspěšně dokončena, když se hello **Dokončit** stavu a jeho **ukončovací kód** je `0`. V Batch .NET, to znamená [CloudTask][net_cloudtask].[ Stav] [ net_taskstate] hodnota vlastnosti `Completed` a hello na CloudTask [TaskExecutionInformation][net_taskexecutioninformation].[ ExitCode] [ net_exitcode] hodnota vlastnosti je `0`.
> 
> 

## <a name="dependency-scenarios"></a>Scénáře závislostí
Existují tři scénáře závislostí základní úlohy, které můžete použít ve službě Azure Batch: 1: 1, 1 n a ID úkolu rozsah závislostí. To mohou být kombinované tooprovide čtvrtý scénáři m: n.

| Scénář&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; | Příklad |  |
|:---:| --- | --- |
|  [1: 1](#one-to-one) |*Úkolb* závisí na *Úkolua* <p/> *Úkolb* nebude naplánovaných pro spuštění až *Úkolua* byla úspěšně dokončena |![Diagram: Úloha 1: 1 závislostí][1] |
|  [Jeden mnoho](#one-to-many) |*ÚkolC* závisí na *úkoluA* a *úkoluB* <p/> *Úkolc* nebude naplánovaných pro spuštění, dokud *Úkolua* a *Úkolb* byly úspěšně dokončeny |![Diagram: závislost na více úkolů][2] |
|  [Rozsah ID úkolu](#task-id-range) |*Úkold* závisí na celou řadu úloh <p/> *Úkold* nebude naplánovaných pro spuštění až hello úlohy s ID *1* prostřednictvím *10* byly úspěšně dokončeny |![Diagram: Úloha id rozsah závislostí][3] |

> [!TIP]
> Můžete vytvořit **m: n** relace, například kde úlohy C, D, E a F každý závisí na úlohy A a B. To je užitečné, například v paralelizovaná málo předběžného zpracování scénáře kde podřízené úkoly závisí na výstup hello několika nadřazeného úloh.
> 
> V příkladech hello v této části závislé úlohy spustí až po hello nadřazené úkoly dokončí úspěšně. Toto chování je hello výchozí chování pro úlohu závislé. Závislé úlohu lze spustit po nadřazené úlohy nepovede zadáním závislostí akce toooverride hello výchozí chování. V tématu hello [závislostí akcí](#dependency-actions) podrobnosti.

### <a name="one-to-one"></a>1: 1
V relaci úloha závisí na hello úspěšné dokončení úlohy jednou nadřazenou položkou. toocreate hello závislostí, zadejte jednu úlohu ID toohello [TaskDependencies][net_taskdependencies].[ OnId] [ net_onid] statickou metodu při naplňování hello [DependsOn] [ net_dependson] vlastnost [CloudTask] [ net_cloudtask].

```csharp
// Task 'taskA' doesn't depend on any other tasks
new CloudTask("taskA", "cmd.exe /c echo taskA"),

// Task 'taskB' depends on completion of task 'taskA'
new CloudTask("taskB", "cmd.exe /c echo taskB")
{
    DependsOn = TaskDependencies.OnId("taskA")
},
```

### <a name="one-to-many"></a>Jeden mnoho
Ve vztahu k více úkol závisí na dokončení hello několika úloh nadřazené. toocreate hello závislostí, zadejte kolekci úloh ID toohello [TaskDependencies][net_taskdependencies].[ OnIds] [ net_onids] statickou metodu při naplňování hello [DependsOn] [ net_dependson] vlastnost [CloudTask] [ net_cloudtask].

```csharp
// 'Rain' and 'Sun' don't depend on any other tasks
new CloudTask("Rain", "cmd.exe /c echo Rain"),
new CloudTask("Sun", "cmd.exe /c echo Sun"),

// Task 'Flowers' depends on completion of both 'Rain' and 'Sun'
// before it is run.
new CloudTask("Flowers", "cmd.exe /c echo Flowers")
{
    DependsOn = TaskDependencies.OnIds("Rain", "Sun")
},
``` 

### <a name="task-id-range"></a>Rozsah ID úkolu
V závislosti na škále nadřazené úlohy úkol závisí na hello hello dokončení úlohy, jejichž identifikátory ležet v rozsahu.
toocreate hello závislostí, zadejte hello první a poslední ID úloh v hello rozsah toohello [TaskDependencies][net_taskdependencies].[ OnIdRange] [ net_onidrange] statickou metodu při naplňování hello [DependsOn] [ net_dependson] vlastnost [CloudTask] [net_cloudtask].

> [!IMPORTANT]
> Pokud použijete rozsahy ID úloh pro svoje závislosti, hello ID úloh v rozsahu hello *musí* být řetězcové vyjádření hodnot celé číslo.
> 
> Každý úkol v rozsahu hello musí splňovat hello závislost, nelze úspěšně dokončit nebo dokončení s chybou, které je namapované tooa závislostí akce nastavit příliš**Satisfy**. V tématu hello [závislostí akcí](#dependency-actions) podrobnosti.
>
>

```csharp
// Tasks 1, 2, and 3 don't depend on any other tasks. Because
// we will be using them for a task range dependency, we must
// specify string representations of integers as their ids.
new CloudTask("1", "cmd.exe /c echo 1"),
new CloudTask("2", "cmd.exe /c echo 2"),
new CloudTask("3", "cmd.exe /c echo 3"),

// Task 4 depends on a range of tasks, 1 through 3
new CloudTask("4", "cmd.exe /c echo 4")
{
    // toouse a range of tasks, their ids must be integer values.
    // Note that we pass integers as parameters tooTaskIdRange,
    // but their ids (above) are string representations of hello ids.
    DependsOn = TaskDependencies.OnIdRange(1, 3)
},
```

## <a name="dependency-actions"></a>Akce závislostí

Ve výchozím nastavení spustí závislé úlohy nebo sadu úloh až po úspěšném dokončení úlohy nadřazené. V některých případech můžete chtít závislé úlohy toorun i v případě selhání úlohy nadřazené hello. Hello výchozí chování můžete přepsat zadáním závislostí akce. Akce závislostí Určuje, zda závislé úlohy oprávněné toorun, na základě hello úspěch nebo selhání úlohy nadřazené hello. 

Předpokládejme například, že závislé úloha čeká na data z hello dokončení hello nadřazený úkol. Pokud nadřazený úkol hello selže, závislé úlohy hello stále může být schopný toorun pomocí starší data. V takovém případě závislostí akce můžete zadat, že tuto úlohu závislé hello je vhodné toorun ohledu na selhání hello hello nadřazené úlohy.

Akce závislostí je založena na ukončovací podmínky pro úlohu nadřazené hello. Můžete zadat akce závislostí pro některý z následujících podmínek ukončení; hello pro platformu .NET, najdete v části hello [ExitConditions] [ net_exitconditions] třída podrobnosti:

- Když dojde k chybě s předem zpracování.
- Když dojde k chybě při odesílání souboru. Pokud úloha hello ukončí s ukončovacím kódem, která byla zadána prostřednictvím **exitCodes** nebo **exitCodeRanges**a pak dojde k chybě při odesílání souboru, hello akci určenou hello ukončovací kód přednost.
- Když úloha hello opustí s ukončovacím kódem definované hello **ExitCodes** vlastnost.
- Když úloha hello opustí s ukončovacím kódem, která spadá do rozsahu určeného hello **ExitCodeRanges** vlastnost.
- Hello případ výchozí, pokud úloha hello ukončí s ukončovacím kódem není definované **ExitCodes** nebo **ExitCodeRanges**, nebo pokud hello úloh ukončí s předem zpracování chyb a hello **PreProcessingError**  není nastavena vlastnost, nebo pokud hello úkolů selhalo s soubor nahrát chyba a hello **FileUploadError** není nastavena vlastnost. 

toospecify závislostí akce v rozhraní .NET, sada hello [ExitOptions][net_exitoptions].[ DependencyAction] [ net_dependencyaction] vlastnost hello ukončovací podmínky. Hello **DependencyAction** vlastnost trvá jednu ze dvou hodnot:

- Nastavení hello **DependencyAction** vlastnost příliš**Satisfy** označuje, že závislé úlohy jsou způsobilé toorun Pokud hello nadřazenou úlohu ukončí kvůli zadané chybě.
- Nastavení hello **DependencyAction** vlastnost příliš**bloku** označuje, že závislé úlohy nejsou způsobilé toorun.

Výchozí nastavení pro hello Hello **DependencyAction** vlastnost je **Satisfy** pro ukončovací kód 0, a **bloku** pro všechny ostatní podmínek ukončení.

Hello následující fragment kódu nastaví hello **DependencyAction** vlastnost pro úlohu nadřazené. Pokud hello nadřazenou úlohu ukončí s předem zpracování chyby nebo s hello hello zadané chybové kódy, závislé úlohy je blokován. Pokud hello nadřazenou úlohu ukončí s nenulovou hodnotou chybě, závislé úlohy hello je vhodné toorun.

```csharp
// Task A is hello parent task.
new CloudTask("A", "cmd.exe /c echo A")
{
    // Specify exit conditions for task A and their dependency actions.
    ExitConditions = new ExitConditions
    {
        // If task A exits with a pre-processing error, block any downstream tasks (in this example, task B).
        PreProcessingError = new ExitOptions
        {
            DependencyAction = DependencyAction.Block
        },
        // If task A exits with hello specified error codes, block any downstream tasks (in this example, task B).
        ExitCodes = new List<ExitCodeMapping>
        {
            new ExitCodeMapping(10, new ExitOptions() { DependencyAction = DependencyAction.Block }),
            new ExitCodeMapping(20, new ExitOptions() { DependencyAction = DependencyAction.Block })
        },
        // If task A succeeds or fails with any other error, any downstream tasks become eligible toorun 
        // (in this example, task B).
        Default = new ExitOptions
        {
            DependencyAction = DependencyAction.Satisfy
        }
    }
},
// Task B depends on task A. Whether it becomes eligible toorun depends on how task A exits.
new CloudTask("B", "cmd.exe /c echo B")
{
    DependsOn = TaskDependencies.OnId("A")
},
```

## <a name="code-sample"></a>Ukázka kódu
Hello [TaskDependencies] [ github_taskdependencies] ukázkový projekt je jedním z hello [ukázky kódu Azure Batch] [ github_samples] na Githubu. Toto řešení sady Visual Studio ukazuje:

- Jak tooenable úkolů závislost na úlohu
- Jak toocreate úkoly, které závisí na jiné úlohy
- Jak tooexecute ty úlohy na fond výpočetních uzlů.

## <a name="next-steps"></a>Další kroky
### <a name="application-deployment"></a>Nasazení aplikace
Hello [balíčky aplikací](batch-application-packages.md) služby Batch poskytuje snadný způsob nasazení tooboth a verze hello aplikace, které vaše úkoly spouští na výpočetních uzlech.

### <a name="installing-applications-and-staging-data"></a>Instalace aplikací a pracovní data
V tématu [instalaci aplikací a dat na Batch pracovních výpočetní uzly] [ forum_post] ve fóru Azure Batch hello přehled při přípravě uzlů toorun úlohy. Pomocí některého z členů týmu Azure Batch hello, zapisovat, že tento příspěvek je dobré Úvod do aplikací toocopy hello různé způsoby, vstupní data úlohy a další soubory tooyour výpočetních uzlů.

[forum_post]: https://social.msdn.microsoft.com/Forums/en-US/87b19671-1bdf-427a-972c-2af7e5ba82d9/installing-applications-and-staging-data-on-batch-compute-nodes?forum=azurebatch
[github_taskdependencies]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/TaskDependencies
[github_samples]: https://github.com/Azure/azure-batch-samples
[net_batchclient]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_cloudjob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_cloudtask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx
[net_dependson]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.dependson.aspx
[net_exitcode]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskexecutioninformation.exitcode.aspx
[net_exitconditions]: https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.exitconditions
[net_exitoptions]: https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.exitoptions
[net_dependencyaction]: https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.exitoptions#Microsoft_Azure_Batch_ExitOptions_DependencyAction
[net_msdn]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[net_onid]: https://msdn.microsoft.com/library/microsoft.azure.batch.taskdependencies.onid.aspx
[net_onids]: https://msdn.microsoft.com/library/microsoft.azure.batch.taskdependencies.onids.aspx
[net_onidrange]: https://msdn.microsoft.com/library/microsoft.azure.batch.taskdependencies.onidrange.aspx
[net_taskexecutioninformation]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskexecutioninformation.aspx
[net_taskstate]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.common.taskstate.aspx
[net_usestaskdependencies]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.usestaskdependencies.aspx
[net_taskdependencies]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskdependencies.aspx

[1]: ./media/batch-task-dependency/01_one_to_one.png "Diagram: závislost 1: 1"
[2]: ./media/batch-task-dependency/02_one_to_many.png "Diagram: závislost na více"
[3]: ./media/batch-task-dependency/03_task_id_range.png "Diagram: Úloha id rozsah závislostí"
