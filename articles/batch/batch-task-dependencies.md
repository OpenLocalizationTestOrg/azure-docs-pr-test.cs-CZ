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
# <a name="create-task-dependencies-toorun-tasks-that-depend-on-other-tasks"></a><span data-ttu-id="488bd-103">Vytvoření závislosti úkolů toorun úlohy, které závisí na jiné úlohy</span><span class="sxs-lookup"><span data-stu-id="488bd-103">Create task dependencies toorun tasks that depend on other tasks</span></span>

<span data-ttu-id="488bd-104">Toorun závislosti úkolů můžete definovat úlohu nebo sadu úloh, až po dokončení úlohy nadřazené.</span><span class="sxs-lookup"><span data-stu-id="488bd-104">You can define task dependencies toorun a task or set of tasks only after a parent task has completed.</span></span> <span data-ttu-id="488bd-105">Některé scénáře, kde jsou užitečné závislosti úkolů patří:</span><span class="sxs-lookup"><span data-stu-id="488bd-105">Some scenarios where task dependencies are useful include:</span></span>

* <span data-ttu-id="488bd-106">Úlohy MapReduce-style v cloudu hello.</span><span class="sxs-lookup"><span data-stu-id="488bd-106">MapReduce-style workloads in hello cloud.</span></span>
* <span data-ttu-id="488bd-107">Úlohy, jejichž úloh zpracování dat může být vyjádřený jako necyklicky (DAG).</span><span class="sxs-lookup"><span data-stu-id="488bd-107">Jobs whose data processing tasks can be expressed as a directed acyclic graph (DAG).</span></span>
* <span data-ttu-id="488bd-108">Vykreslování před a po vykreslení procesy, kde každý úkol musí dokončit před zahájením hello další úlohy.</span><span class="sxs-lookup"><span data-stu-id="488bd-108">Pre-rendering and post-rendering processes, where each task must complete before hello next task can begin.</span></span>
* <span data-ttu-id="488bd-109">Žádné jiné úlohy, ve kterém podřízené úlohy závisí na výstup hello nadřazeného úloh.</span><span class="sxs-lookup"><span data-stu-id="488bd-109">Any other job in which downstream tasks depend on hello output of upstream tasks.</span></span>

<span data-ttu-id="488bd-110">Pomocí závislosti úkolů dávky můžete vytvořit úlohy, které jsou naplánovány pro spuštění na výpočetních uzlech po dokončení jedné nebo více úloh nadřazené hello.</span><span class="sxs-lookup"><span data-stu-id="488bd-110">With Batch task dependencies, you can create tasks that are scheduled for execution on compute nodes after hello completion of one or more parent tasks.</span></span> <span data-ttu-id="488bd-111">Můžete například vytvořit úlohu, která vykreslí každý snímek 3D film samostatný, paralelní úlohy.</span><span class="sxs-lookup"><span data-stu-id="488bd-111">For example, you can create a job that renders each frame of a 3D movie with separate, parallel tasks.</span></span> <span data-ttu-id="488bd-112">Poslední úloha Hello – hello "sloučení úkolů"--sloučení hello vykresluje rámce do hello jenom dokončení film po všech rámců byl úspěšně vykreslen.</span><span class="sxs-lookup"><span data-stu-id="488bd-112">hello final task--hello "merge task"--merges hello rendered frames into hello complete movie only after all frames have been successfully rendered.</span></span>

<span data-ttu-id="488bd-113">Ve výchozím nastavení závislé úlohy jsou naplánovány pro spuštění až po úspěšném dokončení úlohy nadřazené hello.</span><span class="sxs-lookup"><span data-stu-id="488bd-113">By default, dependent tasks are scheduled for execution only after hello parent task has completed successfully.</span></span> <span data-ttu-id="488bd-114">Můžete zadat výchozí chování závislostí akce toooverride hello a spouštět úlohy, pokud úloha nadřazené hello selže.</span><span class="sxs-lookup"><span data-stu-id="488bd-114">You can specify a dependency action toooverride hello default behavior and run tasks when hello parent task fails.</span></span> <span data-ttu-id="488bd-115">V tématu hello [závislostí akcí](#dependency-actions) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="488bd-115">See hello [Dependency actions](#dependency-actions) section for details.</span></span>  

<span data-ttu-id="488bd-116">Můžete vytvořit úlohy, které jsou závislé na dalších úloh v relaci 1: 1 nebo 1 n.</span><span class="sxs-lookup"><span data-stu-id="488bd-116">You can create tasks that depend on other tasks in a one-to-one or one-to-many relationship.</span></span> <span data-ttu-id="488bd-117">Můžete také vytvořit rozsah závislostí, kde úkol závisí na dokončení hello skupiny úloh v rámci zadaného rozsahu ID úkolu.</span><span class="sxs-lookup"><span data-stu-id="488bd-117">You can also create a range dependency where a task depends on hello completion of a group of tasks within a specified range of task IDs.</span></span> <span data-ttu-id="488bd-118">Tyto tři relace m: n toocreate základní scénáře můžete kombinovat.</span><span class="sxs-lookup"><span data-stu-id="488bd-118">You can combine these three basic scenarios toocreate many-to-many relationships.</span></span>

## <a name="task-dependencies-with-batch-net"></a><span data-ttu-id="488bd-119">Závislosti úkolů pomocí rozhraní Batch .NET</span><span class="sxs-lookup"><span data-stu-id="488bd-119">Task dependencies with Batch .NET</span></span>
<span data-ttu-id="488bd-120">V tomto článku probereme, jak závislosti úkolů tooconfigure pomocí hello [Batch .NET] [ net_msdn] knihovny.</span><span class="sxs-lookup"><span data-stu-id="488bd-120">In this article, we discuss how tooconfigure task dependencies by using hello [Batch .NET][net_msdn] library.</span></span> <span data-ttu-id="488bd-121">Nám nejdřív ukazují, jak příliš[povolit závislosti úkolů](#enable-task-dependencies) na úlohách a pak ukazují, jak příliš[úlohu nakonfigurovat závislosti](#create-dependent-tasks).</span><span class="sxs-lookup"><span data-stu-id="488bd-121">We first show you how too[enable task dependency](#enable-task-dependencies) on your jobs, and then demonstrate how too[configure a task with dependencies](#create-dependent-tasks).</span></span> <span data-ttu-id="488bd-122">Také popisují, jak toospecify závislostí akce toorun závislé úlohy Pokud hello nadřazené selže.</span><span class="sxs-lookup"><span data-stu-id="488bd-122">We also describe how toospecify a dependency action toorun dependent tasks if hello parent fails.</span></span> <span data-ttu-id="488bd-123">Nakonec probereme hello [scénáře závislostí](#dependency-scenarios) podporující Batch.</span><span class="sxs-lookup"><span data-stu-id="488bd-123">Finally, we discuss hello [dependency scenarios](#dependency-scenarios) that Batch supports.</span></span>

## <a name="enable-task-dependencies"></a><span data-ttu-id="488bd-124">Povolit závislosti úkolů</span><span class="sxs-lookup"><span data-stu-id="488bd-124">Enable task dependencies</span></span>
<span data-ttu-id="488bd-125">závislosti úkolů toouse ve vaší aplikaci Batch, musíte nejdřív nakonfigurovat závislosti úkolů toouse hello úlohy.</span><span class="sxs-lookup"><span data-stu-id="488bd-125">toouse task dependencies in your Batch application, you must first configure hello job toouse task dependencies.</span></span> <span data-ttu-id="488bd-126">V Batch .NET, povolte ji na vaše [CloudJob] [ net_cloudjob] nastavením jeho [UsesTaskDependencies] [ net_usestaskdependencies] vlastnost příliš`true`:</span><span class="sxs-lookup"><span data-stu-id="488bd-126">In Batch .NET, enable it on your [CloudJob][net_cloudjob] by setting its [UsesTaskDependencies][net_usestaskdependencies] property too`true`:</span></span>

```csharp
CloudJob unboundJob = batchClient.JobOperations.CreateJob( "job001",
    new PoolInformation { PoolId = "pool001" });

// IMPORTANT: This is REQUIRED for using task dependencies.
unboundJob.UsesTaskDependencies = true;
```

<span data-ttu-id="488bd-127">V předchozím fragmentu kódu hello, "batchClient" představuje instanci hello [BatchClient] [ net_batchclient] třídy.</span><span class="sxs-lookup"><span data-stu-id="488bd-127">In hello preceding code snippet, "batchClient" is an instance of hello [BatchClient][net_batchclient] class.</span></span>

## <a name="create-dependent-tasks"></a><span data-ttu-id="488bd-128">Vytvoření závislé úlohy</span><span class="sxs-lookup"><span data-stu-id="488bd-128">Create dependent tasks</span></span>
<span data-ttu-id="488bd-129">toocreate úlohu, která závisí na hello dokončení jedné nebo více úloh nadřazené, můžete zadat, která hello úkolů "závisí na" hello další úlohy.</span><span class="sxs-lookup"><span data-stu-id="488bd-129">toocreate a task that depends on hello completion of one or more parent tasks, you can specify that hello task "depends on" hello other tasks.</span></span> <span data-ttu-id="488bd-130">V Batch .NET, nakonfigurujte hello [CloudTask][net_cloudtask].[ DependsOn] [ net_dependson] vlastnost s instancí hello [TaskDependencies] [ net_taskdependencies] třídy:</span><span class="sxs-lookup"><span data-stu-id="488bd-130">In Batch .NET, configure hello [CloudTask][net_cloudtask].[DependsOn][net_dependson] property with an instance of hello [TaskDependencies][net_taskdependencies] class:</span></span>

```csharp
// Task 'Flowers' depends on completion of both 'Rain' and 'Sun'
// before it is run.
new CloudTask("Flowers", "cmd.exe /c echo Flowers")
{
    DependsOn = TaskDependencies.OnIds("Rain", "Sun")
},
```

<span data-ttu-id="488bd-131">Tento fragment kódu vytvoří závislé úlohy s ID úlohy "Květy".</span><span class="sxs-lookup"><span data-stu-id="488bd-131">This code snippet creates a dependent task with task ID "Flowers".</span></span> <span data-ttu-id="488bd-132">Hello "Květy" úkol závisí na úkoly "Deště" a "Sun".</span><span class="sxs-lookup"><span data-stu-id="488bd-132">hello "Flowers" task depends on tasks "Rain" and "Sun".</span></span> <span data-ttu-id="488bd-133">Úloha "Květy" bude až po úlohy, které byly úspěšně dokončeny "Deště" a "Sun" naplánované toorun na výpočetním uzlu.</span><span class="sxs-lookup"><span data-stu-id="488bd-133">Task "Flowers" will be scheduled toorun on a compute node only after tasks "Rain" and "Sun" have completed successfully.</span></span>

> [!NOTE]
> <span data-ttu-id="488bd-134">Úloha je považován za toobe úspěšně dokončena, když se hello **Dokončit** stavu a jeho **ukončovací kód** je `0`.</span><span class="sxs-lookup"><span data-stu-id="488bd-134">A task is considered toobe completed successfully when it is in hello **completed** state and its **exit code** is `0`.</span></span> <span data-ttu-id="488bd-135">V Batch .NET, to znamená [CloudTask][net_cloudtask].[ Stav] [ net_taskstate] hodnota vlastnosti `Completed` a hello na CloudTask [TaskExecutionInformation][net_taskexecutioninformation].[ ExitCode] [ net_exitcode] hodnota vlastnosti je `0`.</span><span class="sxs-lookup"><span data-stu-id="488bd-135">In Batch .NET, this means a [CloudTask][net_cloudtask].[State][net_taskstate] property value of `Completed` and hello CloudTask's [TaskExecutionInformation][net_taskexecutioninformation].[ExitCode][net_exitcode] property value is `0`.</span></span>
> 
> 

## <a name="dependency-scenarios"></a><span data-ttu-id="488bd-136">Scénáře závislostí</span><span class="sxs-lookup"><span data-stu-id="488bd-136">Dependency scenarios</span></span>
<span data-ttu-id="488bd-137">Existují tři scénáře závislostí základní úlohy, které můžete použít ve službě Azure Batch: 1: 1, 1 n a ID úkolu rozsah závislostí.</span><span class="sxs-lookup"><span data-stu-id="488bd-137">There are three basic task dependency scenarios that you can use in Azure Batch: one-to-one, one-to-many, and task ID range dependency.</span></span> <span data-ttu-id="488bd-138">To mohou být kombinované tooprovide čtvrtý scénáři m: n.</span><span class="sxs-lookup"><span data-stu-id="488bd-138">These can be combined tooprovide a fourth scenario, many-to-many.</span></span>

| <span data-ttu-id="488bd-139">Scénář&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="sxs-lookup"><span data-stu-id="488bd-139">Scenario&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span></span> | <span data-ttu-id="488bd-140">Příklad</span><span class="sxs-lookup"><span data-stu-id="488bd-140">Example</span></span> |  |
|:---:| --- | --- |
|  [<span data-ttu-id="488bd-141">1: 1</span><span class="sxs-lookup"><span data-stu-id="488bd-141">One-to-one</span></span>](#one-to-one) |<span data-ttu-id="488bd-142">*Úkolb* závisí na *Úkolua*</span><span class="sxs-lookup"><span data-stu-id="488bd-142">*taskB* depends on *taskA*</span></span> <p/> <span data-ttu-id="488bd-143">*Úkolb* nebude naplánovaných pro spuštění až *Úkolua* byla úspěšně dokončena</span><span class="sxs-lookup"><span data-stu-id="488bd-143">*taskB* will not be scheduled for execution until *taskA* has completed successfully</span></span> |<span data-ttu-id="488bd-144">![Diagram: Úloha 1: 1 závislostí][1]</span><span class="sxs-lookup"><span data-stu-id="488bd-144">![Diagram: one-to-one task dependency][1]</span></span> |
|  [<span data-ttu-id="488bd-145">Jeden mnoho</span><span class="sxs-lookup"><span data-stu-id="488bd-145">One-to-many</span></span>](#one-to-many) |<span data-ttu-id="488bd-146">*ÚkolC* závisí na *úkoluA* a *úkoluB*</span><span class="sxs-lookup"><span data-stu-id="488bd-146">*taskC* depends on both *taskA* and *taskB*</span></span> <p/> <span data-ttu-id="488bd-147">*Úkolc* nebude naplánovaných pro spuštění, dokud *Úkolua* a *Úkolb* byly úspěšně dokončeny</span><span class="sxs-lookup"><span data-stu-id="488bd-147">*taskC* will not be scheduled for execution until both *taskA* and *taskB* have completed successfully</span></span> |<span data-ttu-id="488bd-148">![Diagram: závislost na více úkolů][2]</span><span class="sxs-lookup"><span data-stu-id="488bd-148">![Diagram: one-to-many task dependency][2]</span></span> |
|  [<span data-ttu-id="488bd-149">Rozsah ID úkolu</span><span class="sxs-lookup"><span data-stu-id="488bd-149">Task ID range</span></span>](#task-id-range) |<span data-ttu-id="488bd-150">*Úkold* závisí na celou řadu úloh</span><span class="sxs-lookup"><span data-stu-id="488bd-150">*taskD* depends on a range of tasks</span></span> <p/> <span data-ttu-id="488bd-151">*Úkold* nebude naplánovaných pro spuštění až hello úlohy s ID *1* prostřednictvím *10* byly úspěšně dokončeny</span><span class="sxs-lookup"><span data-stu-id="488bd-151">*taskD* will not be scheduled for execution until hello tasks with IDs *1* through *10* have completed successfully</span></span> |<span data-ttu-id="488bd-152">![Diagram: Úloha id rozsah závislostí][3]</span><span class="sxs-lookup"><span data-stu-id="488bd-152">![Diagram: Task id range dependency][3]</span></span> |

> [!TIP]
> <span data-ttu-id="488bd-153">Můžete vytvořit **m: n** relace, například kde úlohy C, D, E a F každý závisí na úlohy A a B. To je užitečné, například v paralelizovaná málo předběžného zpracování scénáře kde podřízené úkoly závisí na výstup hello několika nadřazeného úloh.</span><span class="sxs-lookup"><span data-stu-id="488bd-153">You can create **many-to-many** relationships, such as where tasks C, D, E, and F each depend on tasks A and B. This is useful, for example, in parallelized preprocessing scenarios where your downstream tasks depend on hello output of multiple upstream tasks.</span></span>
> 
> <span data-ttu-id="488bd-154">V příkladech hello v této části závislé úlohy spustí až po hello nadřazené úkoly dokončí úspěšně.</span><span class="sxs-lookup"><span data-stu-id="488bd-154">In hello examples in this section, a dependent task runs only after hello parent tasks complete successfully.</span></span> <span data-ttu-id="488bd-155">Toto chování je hello výchozí chování pro úlohu závislé.</span><span class="sxs-lookup"><span data-stu-id="488bd-155">This behavior is hello default behavior for a dependent task.</span></span> <span data-ttu-id="488bd-156">Závislé úlohu lze spustit po nadřazené úlohy nepovede zadáním závislostí akce toooverride hello výchozí chování.</span><span class="sxs-lookup"><span data-stu-id="488bd-156">You can run a dependent task after a parent task fails by specifying a dependency action toooverride hello default behavior.</span></span> <span data-ttu-id="488bd-157">V tématu hello [závislostí akcí](#dependency-actions) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="488bd-157">See hello [Dependency actions](#dependency-actions) section for details.</span></span>

### <a name="one-to-one"></a><span data-ttu-id="488bd-158">1: 1</span><span class="sxs-lookup"><span data-stu-id="488bd-158">One-to-one</span></span>
<span data-ttu-id="488bd-159">V relaci úloha závisí na hello úspěšné dokončení úlohy jednou nadřazenou položkou.</span><span class="sxs-lookup"><span data-stu-id="488bd-159">In a one-to-one relationship, a task depends on hello successful completion of one parent task.</span></span> <span data-ttu-id="488bd-160">toocreate hello závislostí, zadejte jednu úlohu ID toohello [TaskDependencies][net_taskdependencies].[ OnId] [ net_onid] statickou metodu při naplňování hello [DependsOn] [ net_dependson] vlastnost [CloudTask] [ net_cloudtask].</span><span class="sxs-lookup"><span data-stu-id="488bd-160">toocreate hello dependency, provide a single task ID toohello [TaskDependencies][net_taskdependencies].[OnId][net_onid] static method when you populate hello [DependsOn][net_dependson] property of [CloudTask][net_cloudtask].</span></span>

```csharp
// Task 'taskA' doesn't depend on any other tasks
new CloudTask("taskA", "cmd.exe /c echo taskA"),

// Task 'taskB' depends on completion of task 'taskA'
new CloudTask("taskB", "cmd.exe /c echo taskB")
{
    DependsOn = TaskDependencies.OnId("taskA")
},
```

### <a name="one-to-many"></a><span data-ttu-id="488bd-161">Jeden mnoho</span><span class="sxs-lookup"><span data-stu-id="488bd-161">One-to-many</span></span>
<span data-ttu-id="488bd-162">Ve vztahu k více úkol závisí na dokončení hello několika úloh nadřazené.</span><span class="sxs-lookup"><span data-stu-id="488bd-162">In a one-to-many relationship, a task depends on hello completion of multiple parent tasks.</span></span> <span data-ttu-id="488bd-163">toocreate hello závislostí, zadejte kolekci úloh ID toohello [TaskDependencies][net_taskdependencies].[ OnIds] [ net_onids] statickou metodu při naplňování hello [DependsOn] [ net_dependson] vlastnost [CloudTask] [ net_cloudtask].</span><span class="sxs-lookup"><span data-stu-id="488bd-163">toocreate hello dependency, provide a collection of task IDs toohello [TaskDependencies][net_taskdependencies].[OnIds][net_onids] static method when you populate hello [DependsOn][net_dependson] property of [CloudTask][net_cloudtask].</span></span>

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

### <a name="task-id-range"></a><span data-ttu-id="488bd-164">Rozsah ID úkolu</span><span class="sxs-lookup"><span data-stu-id="488bd-164">Task ID range</span></span>
<span data-ttu-id="488bd-165">V závislosti na škále nadřazené úlohy úkol závisí na hello hello dokončení úlohy, jejichž identifikátory ležet v rozsahu.</span><span class="sxs-lookup"><span data-stu-id="488bd-165">In a dependency on a range of parent tasks, a task depends on hello hello completion of tasks whose IDs lie within a range.</span></span>
<span data-ttu-id="488bd-166">toocreate hello závislostí, zadejte hello první a poslední ID úloh v hello rozsah toohello [TaskDependencies][net_taskdependencies].[ OnIdRange] [ net_onidrange] statickou metodu při naplňování hello [DependsOn] [ net_dependson] vlastnost [CloudTask] [net_cloudtask].</span><span class="sxs-lookup"><span data-stu-id="488bd-166">toocreate hello dependency, provide hello first and last task IDs in hello range toohello [TaskDependencies][net_taskdependencies].[OnIdRange][net_onidrange] static method when you populate hello [DependsOn][net_dependson] property of [CloudTask][net_cloudtask].</span></span>

> [!IMPORTANT]
> <span data-ttu-id="488bd-167">Pokud použijete rozsahy ID úloh pro svoje závislosti, hello ID úloh v rozsahu hello *musí* být řetězcové vyjádření hodnot celé číslo.</span><span class="sxs-lookup"><span data-stu-id="488bd-167">When you use task ID ranges for your dependencies, hello task IDs in hello range *must* be string representations of integer values.</span></span>
> 
> <span data-ttu-id="488bd-168">Každý úkol v rozsahu hello musí splňovat hello závislost, nelze úspěšně dokončit nebo dokončení s chybou, které je namapované tooa závislostí akce nastavit příliš**Satisfy**.</span><span class="sxs-lookup"><span data-stu-id="488bd-168">Every task in hello range must satisfy hello dependency, either by completing successfully or by completing with a failure that’s mapped tooa dependency action set too**Satisfy**.</span></span> <span data-ttu-id="488bd-169">V tématu hello [závislostí akcí](#dependency-actions) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="488bd-169">See hello [Dependency actions](#dependency-actions) section for details.</span></span>
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

## <a name="dependency-actions"></a><span data-ttu-id="488bd-170">Akce závislostí</span><span class="sxs-lookup"><span data-stu-id="488bd-170">Dependency actions</span></span>

<span data-ttu-id="488bd-171">Ve výchozím nastavení spustí závislé úlohy nebo sadu úloh až po úspěšném dokončení úlohy nadřazené.</span><span class="sxs-lookup"><span data-stu-id="488bd-171">By default, a dependent task or set of tasks runs only after a parent task has completed successfully.</span></span> <span data-ttu-id="488bd-172">V některých případech můžete chtít závislé úlohy toorun i v případě selhání úlohy nadřazené hello.</span><span class="sxs-lookup"><span data-stu-id="488bd-172">In some scenarios, you may want toorun dependent tasks even if hello parent task fails.</span></span> <span data-ttu-id="488bd-173">Hello výchozí chování můžete přepsat zadáním závislostí akce.</span><span class="sxs-lookup"><span data-stu-id="488bd-173">You can override hello default behavior by specifying a dependency action.</span></span> <span data-ttu-id="488bd-174">Akce závislostí Určuje, zda závislé úlohy oprávněné toorun, na základě hello úspěch nebo selhání úlohy nadřazené hello.</span><span class="sxs-lookup"><span data-stu-id="488bd-174">A dependency action specifies whether a dependent task is eligible toorun, based on hello success or failure of hello parent task.</span></span> 

<span data-ttu-id="488bd-175">Předpokládejme například, že závislé úloha čeká na data z hello dokončení hello nadřazený úkol.</span><span class="sxs-lookup"><span data-stu-id="488bd-175">For example, suppose that a dependent task is awaiting data from hello completion of hello upstream task.</span></span> <span data-ttu-id="488bd-176">Pokud nadřazený úkol hello selže, závislé úlohy hello stále může být schopný toorun pomocí starší data.</span><span class="sxs-lookup"><span data-stu-id="488bd-176">If hello upstream task fails, hello dependent task may still be able toorun using older data.</span></span> <span data-ttu-id="488bd-177">V takovém případě závislostí akce můžete zadat, že tuto úlohu závislé hello je vhodné toorun ohledu na selhání hello hello nadřazené úlohy.</span><span class="sxs-lookup"><span data-stu-id="488bd-177">In this case, a dependency action can specify that hello dependent task is eligible toorun despite hello failure of hello parent task.</span></span>

<span data-ttu-id="488bd-178">Akce závislostí je založena na ukončovací podmínky pro úlohu nadřazené hello.</span><span class="sxs-lookup"><span data-stu-id="488bd-178">A dependency action is based on an exit condition for hello parent task.</span></span> <span data-ttu-id="488bd-179">Můžete zadat akce závislostí pro některý z následujících podmínek ukončení; hello pro platformu .NET, najdete v části hello [ExitConditions] [ net_exitconditions] třída podrobnosti:</span><span class="sxs-lookup"><span data-stu-id="488bd-179">You can specify a dependency action for any of hello following exit conditions; for .NET, see hello [ExitConditions][net_exitconditions] class for details:</span></span>

- <span data-ttu-id="488bd-180">Když dojde k chybě s předem zpracování.</span><span class="sxs-lookup"><span data-stu-id="488bd-180">When a pre-processing error occurs.</span></span>
- <span data-ttu-id="488bd-181">Když dojde k chybě při odesílání souboru.</span><span class="sxs-lookup"><span data-stu-id="488bd-181">When a file upload error occurs.</span></span> <span data-ttu-id="488bd-182">Pokud úloha hello ukončí s ukončovacím kódem, která byla zadána prostřednictvím **exitCodes** nebo **exitCodeRanges**a pak dojde k chybě při odesílání souboru, hello akci určenou hello ukončovací kód přednost.</span><span class="sxs-lookup"><span data-stu-id="488bd-182">If hello task exits with an exit code that was specified via **exitCodes** or **exitCodeRanges**, and then encounters a file upload error, hello action specified by hello exit code takes precedence.</span></span>
- <span data-ttu-id="488bd-183">Když úloha hello opustí s ukončovacím kódem definované hello **ExitCodes** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="488bd-183">When hello task exits with an exit code defined by hello **ExitCodes** property.</span></span>
- <span data-ttu-id="488bd-184">Když úloha hello opustí s ukončovacím kódem, která spadá do rozsahu určeného hello **ExitCodeRanges** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="488bd-184">When hello task exits with an exit code that falls within a range specified by hello **ExitCodeRanges** property.</span></span>
- <span data-ttu-id="488bd-185">Hello případ výchozí, pokud úloha hello ukončí s ukončovacím kódem není definované **ExitCodes** nebo **ExitCodeRanges**, nebo pokud hello úloh ukončí s předem zpracování chyb a hello **PreProcessingError**  není nastavena vlastnost, nebo pokud hello úkolů selhalo s soubor nahrát chyba a hello **FileUploadError** není nastavena vlastnost.</span><span class="sxs-lookup"><span data-stu-id="488bd-185">hello default case, if hello task exits with an exit code not defined by **ExitCodes** or **ExitCodeRanges**, or if hello task exits with a pre-processing error and hello **PreProcessingError** property is not set, or if hello task fails with a file upload error and hello **FileUploadError** property is not set.</span></span> 

<span data-ttu-id="488bd-186">toospecify závislostí akce v rozhraní .NET, sada hello [ExitOptions][net_exitoptions].[ DependencyAction] [ net_dependencyaction] vlastnost hello ukončovací podmínky.</span><span class="sxs-lookup"><span data-stu-id="488bd-186">toospecify a dependency action in .NET, set hello [ExitOptions][net_exitoptions].[DependencyAction][net_dependencyaction] property for hello exit condition.</span></span> <span data-ttu-id="488bd-187">Hello **DependencyAction** vlastnost trvá jednu ze dvou hodnot:</span><span class="sxs-lookup"><span data-stu-id="488bd-187">hello **DependencyAction** property takes one of two values:</span></span>

- <span data-ttu-id="488bd-188">Nastavení hello **DependencyAction** vlastnost příliš**Satisfy** označuje, že závislé úlohy jsou způsobilé toorun Pokud hello nadřazenou úlohu ukončí kvůli zadané chybě.</span><span class="sxs-lookup"><span data-stu-id="488bd-188">Setting hello **DependencyAction** property too**Satisfy** indicates that dependent tasks are eligible toorun if hello parent task exits with a specified error.</span></span>
- <span data-ttu-id="488bd-189">Nastavení hello **DependencyAction** vlastnost příliš**bloku** označuje, že závislé úlohy nejsou způsobilé toorun.</span><span class="sxs-lookup"><span data-stu-id="488bd-189">Setting hello **DependencyAction** property too**Block** indicates that dependent tasks are not eligible toorun.</span></span>

<span data-ttu-id="488bd-190">Výchozí nastavení pro hello Hello **DependencyAction** vlastnost je **Satisfy** pro ukončovací kód 0, a **bloku** pro všechny ostatní podmínek ukončení.</span><span class="sxs-lookup"><span data-stu-id="488bd-190">hello default setting for hello **DependencyAction** property is **Satisfy** for exit code 0, and **Block** for all other exit conditions.</span></span>

<span data-ttu-id="488bd-191">Hello následující fragment kódu nastaví hello **DependencyAction** vlastnost pro úlohu nadřazené.</span><span class="sxs-lookup"><span data-stu-id="488bd-191">hello following code snippet sets hello **DependencyAction** property for a parent task.</span></span> <span data-ttu-id="488bd-192">Pokud hello nadřazenou úlohu ukončí s předem zpracování chyby nebo s hello hello zadané chybové kódy, závislé úlohy je blokován.</span><span class="sxs-lookup"><span data-stu-id="488bd-192">If hello parent task exits with a pre-processing error, or with hello specified error codes, hello dependent task is blocked.</span></span> <span data-ttu-id="488bd-193">Pokud hello nadřazenou úlohu ukončí s nenulovou hodnotou chybě, závislé úlohy hello je vhodné toorun.</span><span class="sxs-lookup"><span data-stu-id="488bd-193">If hello parent task exits with any other non-zero error, hello dependent task is eligible toorun.</span></span>

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

## <a name="code-sample"></a><span data-ttu-id="488bd-194">Ukázka kódu</span><span class="sxs-lookup"><span data-stu-id="488bd-194">Code sample</span></span>
<span data-ttu-id="488bd-195">Hello [TaskDependencies] [ github_taskdependencies] ukázkový projekt je jedním z hello [ukázky kódu Azure Batch] [ github_samples] na Githubu.</span><span class="sxs-lookup"><span data-stu-id="488bd-195">hello [TaskDependencies][github_taskdependencies] sample project is one of hello [Azure Batch code samples][github_samples] on GitHub.</span></span> <span data-ttu-id="488bd-196">Toto řešení sady Visual Studio ukazuje:</span><span class="sxs-lookup"><span data-stu-id="488bd-196">This Visual Studio solution demonstrates:</span></span>

- <span data-ttu-id="488bd-197">Jak tooenable úkolů závislost na úlohu</span><span class="sxs-lookup"><span data-stu-id="488bd-197">How tooenable task dependency on a job</span></span>
- <span data-ttu-id="488bd-198">Jak toocreate úkoly, které závisí na jiné úlohy</span><span class="sxs-lookup"><span data-stu-id="488bd-198">How toocreate tasks that depend on other tasks</span></span>
- <span data-ttu-id="488bd-199">Jak tooexecute ty úlohy na fond výpočetních uzlů.</span><span class="sxs-lookup"><span data-stu-id="488bd-199">How tooexecute those tasks on a pool of compute nodes.</span></span>

## <a name="next-steps"></a><span data-ttu-id="488bd-200">Další kroky</span><span class="sxs-lookup"><span data-stu-id="488bd-200">Next steps</span></span>
### <a name="application-deployment"></a><span data-ttu-id="488bd-201">Nasazení aplikace</span><span class="sxs-lookup"><span data-stu-id="488bd-201">Application deployment</span></span>
<span data-ttu-id="488bd-202">Hello [balíčky aplikací](batch-application-packages.md) služby Batch poskytuje snadný způsob nasazení tooboth a verze hello aplikace, které vaše úkoly spouští na výpočetních uzlech.</span><span class="sxs-lookup"><span data-stu-id="488bd-202">hello [application packages](batch-application-packages.md) feature of Batch provides an easy way tooboth deploy and version hello applications that your tasks execute on compute nodes.</span></span>

### <a name="installing-applications-and-staging-data"></a><span data-ttu-id="488bd-203">Instalace aplikací a pracovní data</span><span class="sxs-lookup"><span data-stu-id="488bd-203">Installing applications and staging data</span></span>
<span data-ttu-id="488bd-204">V tématu [instalaci aplikací a dat na Batch pracovních výpočetní uzly] [ forum_post] ve fóru Azure Batch hello přehled při přípravě uzlů toorun úlohy.</span><span class="sxs-lookup"><span data-stu-id="488bd-204">See [Installing applications and staging data on Batch compute nodes][forum_post] in hello Azure Batch forum for an overview of methods for preparing your nodes toorun tasks.</span></span> <span data-ttu-id="488bd-205">Pomocí některého z členů týmu Azure Batch hello, zapisovat, že tento příspěvek je dobré Úvod do aplikací toocopy hello různé způsoby, vstupní data úlohy a další soubory tooyour výpočetních uzlů.</span><span class="sxs-lookup"><span data-stu-id="488bd-205">Written by one of hello Azure Batch team members, this post is a good primer on hello different ways toocopy applications, task input data, and other files tooyour compute nodes.</span></span>

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
