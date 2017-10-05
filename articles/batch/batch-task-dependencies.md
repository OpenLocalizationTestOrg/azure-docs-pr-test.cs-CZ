---
title: "Pomocí závislosti úkolů spouštět úlohy, které jsou založeny na dokončení jiné úlohy – Azure Batch | Microsoft Docs"
description: "Vytvoření úlohy, které jsou závislé na dokončení jiné úlohy pro zpracování prostředí MapReduce style a podobné velkých objemů dat úlohy v Azure Batch."
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
ms.openlocfilehash: 465306d2de8d1dbe6ba1f0cd74be720b78a50de3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="create-task-dependencies-to-run-tasks-that-depend-on-other-tasks"></a><span data-ttu-id="1d657-103">Vytvoření závislosti úkolů ke spouštění úloh, které závisí na jiné úlohy</span><span class="sxs-lookup"><span data-stu-id="1d657-103">Create task dependencies to run tasks that depend on other tasks</span></span>

<span data-ttu-id="1d657-104">Můžete definovat závislosti úkolů pro spuštění úloh nebo sadu úloh, až po dokončení úlohy nadřazené.</span><span class="sxs-lookup"><span data-stu-id="1d657-104">You can define task dependencies to run a task or set of tasks only after a parent task has completed.</span></span> <span data-ttu-id="1d657-105">Některé scénáře, kde jsou užitečné závislosti úkolů patří:</span><span class="sxs-lookup"><span data-stu-id="1d657-105">Some scenarios where task dependencies are useful include:</span></span>

* <span data-ttu-id="1d657-106">Úlohy MapReduce-style v cloudu.</span><span class="sxs-lookup"><span data-stu-id="1d657-106">MapReduce-style workloads in the cloud.</span></span>
* <span data-ttu-id="1d657-107">Úlohy, jejichž úloh zpracování dat může být vyjádřený jako necyklicky (DAG).</span><span class="sxs-lookup"><span data-stu-id="1d657-107">Jobs whose data processing tasks can be expressed as a directed acyclic graph (DAG).</span></span>
* <span data-ttu-id="1d657-108">Zahájit vykreslování před a po vykreslení procesy, kde musí každý úkol dokončit před dalším úkolem.</span><span class="sxs-lookup"><span data-stu-id="1d657-108">Pre-rendering and post-rendering processes, where each task must complete before the next task can begin.</span></span>
* <span data-ttu-id="1d657-109">Žádné jiné úlohy, ve kterém podřízené úlohy závisí na výstup úlohy nadřazený.</span><span class="sxs-lookup"><span data-stu-id="1d657-109">Any other job in which downstream tasks depend on the output of upstream tasks.</span></span>

<span data-ttu-id="1d657-110">Pomocí závislosti úkolů dávky můžete vytvořit úlohy, které jsou naplánovány pro spuštění na výpočetních uzlech po dokončení jedné nebo více úloh nadřazené.</span><span class="sxs-lookup"><span data-stu-id="1d657-110">With Batch task dependencies, you can create tasks that are scheduled for execution on compute nodes after the completion of one or more parent tasks.</span></span> <span data-ttu-id="1d657-111">Můžete například vytvořit úlohu, která vykreslí každý snímek 3D film samostatný, paralelní úlohy.</span><span class="sxs-lookup"><span data-stu-id="1d657-111">For example, you can create a job that renders each frame of a 3D movie with separate, parallel tasks.</span></span> <span data-ttu-id="1d657-112">Konečné úloh – "sloučení úlohu"--sloučení vykreslené rámce do dokončení film až poté, co byl úspěšně vykreslen všechny snímky.</span><span class="sxs-lookup"><span data-stu-id="1d657-112">The final task--the "merge task"--merges the rendered frames into the complete movie only after all frames have been successfully rendered.</span></span>

<span data-ttu-id="1d657-113">Ve výchozím nastavení závislé úlohy jsou naplánovány pro spuštění až po úspěšném dokončení úlohy nadřazené.</span><span class="sxs-lookup"><span data-stu-id="1d657-113">By default, dependent tasks are scheduled for execution only after the parent task has completed successfully.</span></span> <span data-ttu-id="1d657-114">Můžete zadat akce závislostí přepsat výchozí chování a spouštět úlohy, pokud úloha nadřazené selže.</span><span class="sxs-lookup"><span data-stu-id="1d657-114">You can specify a dependency action to override the default behavior and run tasks when the parent task fails.</span></span> <span data-ttu-id="1d657-115">Najdete v článku [závislostí akcí](#dependency-actions) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="1d657-115">See the [Dependency actions](#dependency-actions) section for details.</span></span>  

<span data-ttu-id="1d657-116">Můžete vytvořit úlohy, které jsou závislé na dalších úloh v relaci 1: 1 nebo 1 n.</span><span class="sxs-lookup"><span data-stu-id="1d657-116">You can create tasks that depend on other tasks in a one-to-one or one-to-many relationship.</span></span> <span data-ttu-id="1d657-117">Můžete také vytvořit rozsah závislostí, kde úkol závisí na dokončení úloh v zadaném rozsahu úlohy ID skupiny.</span><span class="sxs-lookup"><span data-stu-id="1d657-117">You can also create a range dependency where a task depends on the completion of a group of tasks within a specified range of task IDs.</span></span> <span data-ttu-id="1d657-118">Můžete kombinovat těchto tří základních scénářů pro vytvoření relace m: n.</span><span class="sxs-lookup"><span data-stu-id="1d657-118">You can combine these three basic scenarios to create many-to-many relationships.</span></span>

## <a name="task-dependencies-with-batch-net"></a><span data-ttu-id="1d657-119">Závislosti úkolů pomocí rozhraní Batch .NET</span><span class="sxs-lookup"><span data-stu-id="1d657-119">Task dependencies with Batch .NET</span></span>
<span data-ttu-id="1d657-120">V tomto článku probereme, jak nakonfigurovat závislosti úkolů pomocí [Batch .NET] [ net_msdn] knihovny.</span><span class="sxs-lookup"><span data-stu-id="1d657-120">In this article, we discuss how to configure task dependencies by using the [Batch .NET][net_msdn] library.</span></span> <span data-ttu-id="1d657-121">Nám nejdřív ukazují, jak k [povolit závislosti úkolů](#enable-task-dependencies) na úlohách a pak ukazují, jak [úlohu nakonfigurovat závislosti](#create-dependent-tasks).</span><span class="sxs-lookup"><span data-stu-id="1d657-121">We first show you how to [enable task dependency](#enable-task-dependencies) on your jobs, and then demonstrate how to [configure a task with dependencies](#create-dependent-tasks).</span></span> <span data-ttu-id="1d657-122">Můžeme také popisují, jak určení závislostí akce ke spouštění závislé úlohy, pokud nadřazená selže.</span><span class="sxs-lookup"><span data-stu-id="1d657-122">We also describe how to specify a dependency action to run dependent tasks if the parent fails.</span></span> <span data-ttu-id="1d657-123">Nakonec probereme [scénáře závislostí](#dependency-scenarios) podporující Batch.</span><span class="sxs-lookup"><span data-stu-id="1d657-123">Finally, we discuss the [dependency scenarios](#dependency-scenarios) that Batch supports.</span></span>

## <a name="enable-task-dependencies"></a><span data-ttu-id="1d657-124">Povolit závislosti úkolů</span><span class="sxs-lookup"><span data-stu-id="1d657-124">Enable task dependencies</span></span>
<span data-ttu-id="1d657-125">Používat závislosti úkolů v aplikaci Batch, musíte nejdřív nakonfigurovat úlohy pomocí závislosti úkolů.</span><span class="sxs-lookup"><span data-stu-id="1d657-125">To use task dependencies in your Batch application, you must first configure the job to use task dependencies.</span></span> <span data-ttu-id="1d657-126">V Batch .NET, povolte ji na vaše [CloudJob] [ net_cloudjob] nastavením jeho [UsesTaskDependencies] [ net_usestaskdependencies] vlastnost `true`:</span><span class="sxs-lookup"><span data-stu-id="1d657-126">In Batch .NET, enable it on your [CloudJob][net_cloudjob] by setting its [UsesTaskDependencies][net_usestaskdependencies] property to `true`:</span></span>

```csharp
CloudJob unboundJob = batchClient.JobOperations.CreateJob( "job001",
    new PoolInformation { PoolId = "pool001" });

// IMPORTANT: This is REQUIRED for using task dependencies.
unboundJob.UsesTaskDependencies = true;
```

<span data-ttu-id="1d657-127">V předchozím fragmentu kódu je "batchClient" instance [BatchClient] [ net_batchclient] třídy.</span><span class="sxs-lookup"><span data-stu-id="1d657-127">In the preceding code snippet, "batchClient" is an instance of the [BatchClient][net_batchclient] class.</span></span>

## <a name="create-dependent-tasks"></a><span data-ttu-id="1d657-128">Vytvoření závislé úlohy</span><span class="sxs-lookup"><span data-stu-id="1d657-128">Create dependent tasks</span></span>
<span data-ttu-id="1d657-129">Chcete-li vytvořit úlohu, která závisí na dokončení jedné nebo více úloh nadřazené, můžete zadat, že úloha "závisí na" Další úlohy.</span><span class="sxs-lookup"><span data-stu-id="1d657-129">To create a task that depends on the completion of one or more parent tasks, you can specify that the task "depends on" the other tasks.</span></span> <span data-ttu-id="1d657-130">V Batch .NET, nakonfigurujte [CloudTask][net_cloudtask].[ DependsOn] [ net_dependson] vlastnost s instancí [TaskDependencies] [ net_taskdependencies] třídy:</span><span class="sxs-lookup"><span data-stu-id="1d657-130">In Batch .NET, configure the [CloudTask][net_cloudtask].[DependsOn][net_dependson] property with an instance of the [TaskDependencies][net_taskdependencies] class:</span></span>

```csharp
// Task 'Flowers' depends on completion of both 'Rain' and 'Sun'
// before it is run.
new CloudTask("Flowers", "cmd.exe /c echo Flowers")
{
    DependsOn = TaskDependencies.OnIds("Rain", "Sun")
},
```

<span data-ttu-id="1d657-131">Tento fragment kódu vytvoří závislé úlohy s ID úlohy "Květy".</span><span class="sxs-lookup"><span data-stu-id="1d657-131">This code snippet creates a dependent task with task ID "Flowers".</span></span> <span data-ttu-id="1d657-132">Úloha "Květy" závisí na úkoly "Deště" a "Sun".</span><span class="sxs-lookup"><span data-stu-id="1d657-132">The "Flowers" task depends on tasks "Rain" and "Sun".</span></span> <span data-ttu-id="1d657-133">Úloha "květy" bude naplánovat na spuštění na výpočetním uzlu až po úlohy, které byly úspěšně dokončeny "Deště" a "Sun".</span><span class="sxs-lookup"><span data-stu-id="1d657-133">Task "Flowers" will be scheduled to run on a compute node only after tasks "Rain" and "Sun" have completed successfully.</span></span>

> [!NOTE]
> <span data-ttu-id="1d657-134">Úloha se považuje za úspěšně dokončit, pokud je v **Dokončit** stavu a jeho **ukončovací kód** je `0`.</span><span class="sxs-lookup"><span data-stu-id="1d657-134">A task is considered to be completed successfully when it is in the **completed** state and its **exit code** is `0`.</span></span> <span data-ttu-id="1d657-135">V Batch .NET, to znamená [CloudTask][net_cloudtask].[ Stav] [ net_taskstate] hodnota vlastnosti `Completed` a CloudTask [TaskExecutionInformation][net_taskexecutioninformation].[ ExitCode] [ net_exitcode] hodnota vlastnosti je `0`.</span><span class="sxs-lookup"><span data-stu-id="1d657-135">In Batch .NET, this means a [CloudTask][net_cloudtask].[State][net_taskstate] property value of `Completed` and the CloudTask's [TaskExecutionInformation][net_taskexecutioninformation].[ExitCode][net_exitcode] property value is `0`.</span></span>
> 
> 

## <a name="dependency-scenarios"></a><span data-ttu-id="1d657-136">Scénáře závislostí</span><span class="sxs-lookup"><span data-stu-id="1d657-136">Dependency scenarios</span></span>
<span data-ttu-id="1d657-137">Existují tři scénáře závislostí základní úlohy, které můžete použít ve službě Azure Batch: 1: 1, 1 n a ID úkolu rozsah závislostí.</span><span class="sxs-lookup"><span data-stu-id="1d657-137">There are three basic task dependency scenarios that you can use in Azure Batch: one-to-one, one-to-many, and task ID range dependency.</span></span> <span data-ttu-id="1d657-138">Ty mohou být kombinovány zajistit scénáři čtvrtý m: n.</span><span class="sxs-lookup"><span data-stu-id="1d657-138">These can be combined to provide a fourth scenario, many-to-many.</span></span>

| <span data-ttu-id="1d657-139">Scénář&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span><span class="sxs-lookup"><span data-stu-id="1d657-139">Scenario&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span></span> | <span data-ttu-id="1d657-140">Příklad</span><span class="sxs-lookup"><span data-stu-id="1d657-140">Example</span></span> |  |
|:---:| --- | --- |
|  [<span data-ttu-id="1d657-141">1: 1</span><span class="sxs-lookup"><span data-stu-id="1d657-141">One-to-one</span></span>](#one-to-one) |<span data-ttu-id="1d657-142">*Úkolb* závisí na *Úkolua*</span><span class="sxs-lookup"><span data-stu-id="1d657-142">*taskB* depends on *taskA*</span></span> <p/> <span data-ttu-id="1d657-143">*Úkolb* nebude naplánovaných pro spuštění až *Úkolua* byla úspěšně dokončena</span><span class="sxs-lookup"><span data-stu-id="1d657-143">*taskB* will not be scheduled for execution until *taskA* has completed successfully</span></span> |<span data-ttu-id="1d657-144">![Diagram: Úloha 1: 1 závislostí][1]</span><span class="sxs-lookup"><span data-stu-id="1d657-144">![Diagram: one-to-one task dependency][1]</span></span> |
|  [<span data-ttu-id="1d657-145">Jeden mnoho</span><span class="sxs-lookup"><span data-stu-id="1d657-145">One-to-many</span></span>](#one-to-many) |<span data-ttu-id="1d657-146">*ÚkolC* závisí na *úkoluA* a *úkoluB*</span><span class="sxs-lookup"><span data-stu-id="1d657-146">*taskC* depends on both *taskA* and *taskB*</span></span> <p/> <span data-ttu-id="1d657-147">*Úkolc* nebude naplánovaných pro spuštění, dokud *Úkolua* a *Úkolb* byly úspěšně dokončeny</span><span class="sxs-lookup"><span data-stu-id="1d657-147">*taskC* will not be scheduled for execution until both *taskA* and *taskB* have completed successfully</span></span> |<span data-ttu-id="1d657-148">![Diagram: závislost na více úkolů][2]</span><span class="sxs-lookup"><span data-stu-id="1d657-148">![Diagram: one-to-many task dependency][2]</span></span> |
|  [<span data-ttu-id="1d657-149">Rozsah ID úkolu</span><span class="sxs-lookup"><span data-stu-id="1d657-149">Task ID range</span></span>](#task-id-range) |<span data-ttu-id="1d657-150">*Úkold* závisí na celou řadu úloh</span><span class="sxs-lookup"><span data-stu-id="1d657-150">*taskD* depends on a range of tasks</span></span> <p/> <span data-ttu-id="1d657-151">*Úkold* nebude naplánovaných pro spuštění až úlohy s ID *1* prostřednictvím *10* byly úspěšně dokončeny</span><span class="sxs-lookup"><span data-stu-id="1d657-151">*taskD* will not be scheduled for execution until the tasks with IDs *1* through *10* have completed successfully</span></span> |<span data-ttu-id="1d657-152">![Diagram: Úloha id rozsah závislostí][3]</span><span class="sxs-lookup"><span data-stu-id="1d657-152">![Diagram: Task id range dependency][3]</span></span> |

> [!TIP]
> <span data-ttu-id="1d657-153">Můžete vytvořit **m: n** relace, například kde úlohy C, D, E a F každý závisí na úlohy A a B. To je užitečné, například v paralelizovaná málo předběžného zpracování scénáře kde podřízené úkoly závisí na výstupu několika nadřazeného úloh.</span><span class="sxs-lookup"><span data-stu-id="1d657-153">You can create **many-to-many** relationships, such as where tasks C, D, E, and F each depend on tasks A and B. This is useful, for example, in parallelized preprocessing scenarios where your downstream tasks depend on the output of multiple upstream tasks.</span></span>
> 
> <span data-ttu-id="1d657-154">V příkladech v této části závislé úlohy spustí až po nadřazené úlohy úspěšně dokončit.</span><span class="sxs-lookup"><span data-stu-id="1d657-154">In the examples in this section, a dependent task runs only after the parent tasks complete successfully.</span></span> <span data-ttu-id="1d657-155">Toto chování je výchozí chování pro úlohu závislé.</span><span class="sxs-lookup"><span data-stu-id="1d657-155">This behavior is the default behavior for a dependent task.</span></span> <span data-ttu-id="1d657-156">Závislé úlohu lze spustit po nadřazené úlohy nepovede zadáním závislostí akce přepsat výchozí chování.</span><span class="sxs-lookup"><span data-stu-id="1d657-156">You can run a dependent task after a parent task fails by specifying a dependency action to override the default behavior.</span></span> <span data-ttu-id="1d657-157">Najdete v článku [závislostí akcí](#dependency-actions) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="1d657-157">See the [Dependency actions](#dependency-actions) section for details.</span></span>

### <a name="one-to-one"></a><span data-ttu-id="1d657-158">1: 1</span><span class="sxs-lookup"><span data-stu-id="1d657-158">One-to-one</span></span>
<span data-ttu-id="1d657-159">V relaci úkol závisí na úspěšné dokončení úlohy jednou nadřazenou položkou.</span><span class="sxs-lookup"><span data-stu-id="1d657-159">In a one-to-one relationship, a task depends on the successful completion of one parent task.</span></span> <span data-ttu-id="1d657-160">Pokud chcete vytvořit závislost, uveďte ID jedné úlohy pro [TaskDependencies][net_taskdependencies].[ OnId] [ net_onid] statickou metodu při naplňování [DependsOn] [ net_dependson] vlastnost [CloudTask][net_cloudtask].</span><span class="sxs-lookup"><span data-stu-id="1d657-160">To create the dependency, provide a single task ID to the [TaskDependencies][net_taskdependencies].[OnId][net_onid] static method when you populate the [DependsOn][net_dependson] property of [CloudTask][net_cloudtask].</span></span>

```csharp
// Task 'taskA' doesn't depend on any other tasks
new CloudTask("taskA", "cmd.exe /c echo taskA"),

// Task 'taskB' depends on completion of task 'taskA'
new CloudTask("taskB", "cmd.exe /c echo taskB")
{
    DependsOn = TaskDependencies.OnId("taskA")
},
```

### <a name="one-to-many"></a><span data-ttu-id="1d657-161">Jeden mnoho</span><span class="sxs-lookup"><span data-stu-id="1d657-161">One-to-many</span></span>
<span data-ttu-id="1d657-162">Ve vztahu k více úkol závisí na dokončení více úkolů nadřazené.</span><span class="sxs-lookup"><span data-stu-id="1d657-162">In a one-to-many relationship, a task depends on the completion of multiple parent tasks.</span></span> <span data-ttu-id="1d657-163">Pokud chcete vytvořit závislost, shrnují ID úloh na [TaskDependencies][net_taskdependencies].[ OnIds] [ net_onids] statickou metodu při naplňování [DependsOn] [ net_dependson] vlastnost [CloudTask][net_cloudtask].</span><span class="sxs-lookup"><span data-stu-id="1d657-163">To create the dependency, provide a collection of task IDs to the [TaskDependencies][net_taskdependencies].[OnIds][net_onids] static method when you populate the [DependsOn][net_dependson] property of [CloudTask][net_cloudtask].</span></span>

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

### <a name="task-id-range"></a><span data-ttu-id="1d657-164">Rozsah ID úkolu</span><span class="sxs-lookup"><span data-stu-id="1d657-164">Task ID range</span></span>
<span data-ttu-id="1d657-165">V závislosti na škále nadřazené úlohy, úkol závisí dokončení úlohy, jejichž identifikátory ležet v rozsahu.</span><span class="sxs-lookup"><span data-stu-id="1d657-165">In a dependency on a range of parent tasks, a task depends on the the completion of tasks whose IDs lie within a range.</span></span>
<span data-ttu-id="1d657-166">Pokud chcete vytvořit závislost, zadejte první a poslední ID úloh v rozsahu do [TaskDependencies][net_taskdependencies].[ OnIdRange] [ net_onidrange] statickou metodu při naplňování [DependsOn] [ net_dependson] vlastnost [CloudTask][net_cloudtask].</span><span class="sxs-lookup"><span data-stu-id="1d657-166">To create the dependency, provide the first and last task IDs in the range to the [TaskDependencies][net_taskdependencies].[OnIdRange][net_onidrange] static method when you populate the [DependsOn][net_dependson] property of [CloudTask][net_cloudtask].</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1d657-167">Když použijete rozsahy ID úloh pro závislosti, ID úlohy v rozsahu *musí* být řetězcové vyjádření hodnot celé číslo.</span><span class="sxs-lookup"><span data-stu-id="1d657-167">When you use task ID ranges for your dependencies, the task IDs in the range *must* be string representations of integer values.</span></span>
> 
> <span data-ttu-id="1d657-168">Každý úkol v rozsahu musí splňovat závislost, nelze úspěšně dokončit nebo dokončení s chybou, který je namapovaný na závislostí akcí nastavenou na hodnotu **Satisfy**.</span><span class="sxs-lookup"><span data-stu-id="1d657-168">Every task in the range must satisfy the dependency, either by completing successfully or by completing with a failure that’s mapped to a dependency action set to **Satisfy**.</span></span> <span data-ttu-id="1d657-169">Najdete v článku [závislostí akcí](#dependency-actions) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="1d657-169">See the [Dependency actions](#dependency-actions) section for details.</span></span>
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
    // To use a range of tasks, their ids must be integer values.
    // Note that we pass integers as parameters to TaskIdRange,
    // but their ids (above) are string representations of the ids.
    DependsOn = TaskDependencies.OnIdRange(1, 3)
},
```

## <a name="dependency-actions"></a><span data-ttu-id="1d657-170">Akce závislostí</span><span class="sxs-lookup"><span data-stu-id="1d657-170">Dependency actions</span></span>

<span data-ttu-id="1d657-171">Ve výchozím nastavení spustí závislé úlohy nebo sadu úloh až po úspěšném dokončení úlohy nadřazené.</span><span class="sxs-lookup"><span data-stu-id="1d657-171">By default, a dependent task or set of tasks runs only after a parent task has completed successfully.</span></span> <span data-ttu-id="1d657-172">V některých scénářích můžete spustit závislé úlohy i v případě, že úloha nadřazené nezdaří.</span><span class="sxs-lookup"><span data-stu-id="1d657-172">In some scenarios, you may want to run dependent tasks even if the parent task fails.</span></span> <span data-ttu-id="1d657-173">Výchozí chování můžete přepsat zadáním závislostí akce.</span><span class="sxs-lookup"><span data-stu-id="1d657-173">You can override the default behavior by specifying a dependency action.</span></span> <span data-ttu-id="1d657-174">Akce závislostí Určuje, jestli závislé úlohy je vhodné spustit, na základě úspěch nebo selhání úlohy nadřazené.</span><span class="sxs-lookup"><span data-stu-id="1d657-174">A dependency action specifies whether a dependent task is eligible to run, based on the success or failure of the parent task.</span></span> 

<span data-ttu-id="1d657-175">Předpokládejme například, že závislé úloha čeká na data z dokončení nadřazený úkol.</span><span class="sxs-lookup"><span data-stu-id="1d657-175">For example, suppose that a dependent task is awaiting data from the completion of the upstream task.</span></span> <span data-ttu-id="1d657-176">Pokud nadřazený úkol selže, může být závislé úlohy stále moci spouštět pomocí starší data.</span><span class="sxs-lookup"><span data-stu-id="1d657-176">If the upstream task fails, the dependent task may still be able to run using older data.</span></span> <span data-ttu-id="1d657-177">Akce závislostí v tomto případě můžete zadat, že závislé úkol je vhodné spustit bez ohledu na selhání nadřazené úlohy.</span><span class="sxs-lookup"><span data-stu-id="1d657-177">In this case, a dependency action can specify that the dependent task is eligible to run despite the failure of the parent task.</span></span>

<span data-ttu-id="1d657-178">Akce závislostí je založena na ukončovací podmínky pro nadřazené úlohy.</span><span class="sxs-lookup"><span data-stu-id="1d657-178">A dependency action is based on an exit condition for the parent task.</span></span> <span data-ttu-id="1d657-179">Můžete zadat akce závislostí pro některý z následujících podmínek ukončení; pro platformu .NET, najdete v článku [ExitConditions] [ net_exitconditions] třída podrobnosti:</span><span class="sxs-lookup"><span data-stu-id="1d657-179">You can specify a dependency action for any of the following exit conditions; for .NET, see the [ExitConditions][net_exitconditions] class for details:</span></span>

- <span data-ttu-id="1d657-180">Když dojde k chybě s předem zpracování.</span><span class="sxs-lookup"><span data-stu-id="1d657-180">When a pre-processing error occurs.</span></span>
- <span data-ttu-id="1d657-181">Když dojde k chybě při odesílání souboru.</span><span class="sxs-lookup"><span data-stu-id="1d657-181">When a file upload error occurs.</span></span> <span data-ttu-id="1d657-182">Pokud úloha ukončení s ukončovacím kódem, která byla zadána prostřednictvím **exitCodes** nebo **exitCodeRanges**, a pak mají přednost komunikaci soubor nahrát chyba akci určenou v něm ukončovací kód.</span><span class="sxs-lookup"><span data-stu-id="1d657-182">If the task exits with an exit code that was specified via **exitCodes** or **exitCodeRanges**, and then encounters a file upload error, the action specified by the exit code takes precedence.</span></span>
- <span data-ttu-id="1d657-183">Při ukončení úlohy s ukončovacím kódem definované **ExitCodes** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="1d657-183">When the task exits with an exit code defined by the **ExitCodes** property.</span></span>
- <span data-ttu-id="1d657-184">Při ukončení úlohy s ukončovacím kódem, která spadá do rozsahu určeného **ExitCodeRanges** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="1d657-184">When the task exits with an exit code that falls within a range specified by the **ExitCodeRanges** property.</span></span>
- <span data-ttu-id="1d657-185">Výchozí Ano, pokud úlohu ukončí s ukončovacím kódem není definované **ExitCodes** nebo **ExitCodeRanges**, nebo pokud úlohu ukončí s předem zpracování chyb a **PreProcessingError** není nastavena vlastnost, nebo pokud se úloha nezdaří s soubor nahrát chyba a **FileUploadError** není nastavena vlastnost.</span><span class="sxs-lookup"><span data-stu-id="1d657-185">The default case, if the task exits with an exit code not defined by **ExitCodes** or **ExitCodeRanges**, or if the task exits with a pre-processing error and the **PreProcessingError** property is not set, or if the task fails with a file upload error and the **FileUploadError** property is not set.</span></span> 

<span data-ttu-id="1d657-186">Určení závislostí akce v rozhraní .NET, nastavte [ExitOptions][net_exitoptions].[ DependencyAction] [ net_dependencyaction] vlastnost ukončovací podmínky.</span><span class="sxs-lookup"><span data-stu-id="1d657-186">To specify a dependency action in .NET, set the [ExitOptions][net_exitoptions].[DependencyAction][net_dependencyaction] property for the exit condition.</span></span> <span data-ttu-id="1d657-187">**DependencyAction** vlastnost trvá jednu ze dvou hodnot:</span><span class="sxs-lookup"><span data-stu-id="1d657-187">The **DependencyAction** property takes one of two values:</span></span>

- <span data-ttu-id="1d657-188">Nastavení **DependencyAction** vlastnost **Satisfy** označuje, že závislé úlohy jsou způsobilé ke spouštění, pokud nadřazená úloha ukončí kvůli zadané chybě.</span><span class="sxs-lookup"><span data-stu-id="1d657-188">Setting the **DependencyAction** property to **Satisfy** indicates that dependent tasks are eligible to run if the parent task exits with a specified error.</span></span>
- <span data-ttu-id="1d657-189">Nastavení **DependencyAction** vlastnost **bloku** označuje, že závislé úlohy nejsou způsobilé ke spuštění.</span><span class="sxs-lookup"><span data-stu-id="1d657-189">Setting the **DependencyAction** property to **Block** indicates that dependent tasks are not eligible to run.</span></span>

<span data-ttu-id="1d657-190">Výchozí nastavení **DependencyAction** vlastnost je **Satisfy** pro ukončovací kód 0, a **bloku** pro všechny ostatní podmínek ukončení.</span><span class="sxs-lookup"><span data-stu-id="1d657-190">The default setting for the **DependencyAction** property is **Satisfy** for exit code 0, and **Block** for all other exit conditions.</span></span>

<span data-ttu-id="1d657-191">Následující fragment kódu nastaví kód **DependencyAction** vlastnost pro úlohu nadřazené.</span><span class="sxs-lookup"><span data-stu-id="1d657-191">The following code snippet sets the **DependencyAction** property for a parent task.</span></span> <span data-ttu-id="1d657-192">Pokud úloha nadřazené ukončí předběžného zpracování chyby nebo s zadané chybové kódy, závislé úlohy je blokován.</span><span class="sxs-lookup"><span data-stu-id="1d657-192">If the parent task exits with a pre-processing error, or with the specified error codes, the dependent task is blocked.</span></span> <span data-ttu-id="1d657-193">Pokud úloha nadřazené ukončí s nenulovou hodnotou chybě, závislé úlohy nemá oprávnění ke spuštění.</span><span class="sxs-lookup"><span data-stu-id="1d657-193">If the parent task exits with any other non-zero error, the dependent task is eligible to run.</span></span>

```csharp
// Task A is the parent task.
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
        // If task A exits with the specified error codes, block any downstream tasks (in this example, task B).
        ExitCodes = new List<ExitCodeMapping>
        {
            new ExitCodeMapping(10, new ExitOptions() { DependencyAction = DependencyAction.Block }),
            new ExitCodeMapping(20, new ExitOptions() { DependencyAction = DependencyAction.Block })
        },
        // If task A succeeds or fails with any other error, any downstream tasks become eligible to run 
        // (in this example, task B).
        Default = new ExitOptions
        {
            DependencyAction = DependencyAction.Satisfy
        }
    }
},
// Task B depends on task A. Whether it becomes eligible to run depends on how task A exits.
new CloudTask("B", "cmd.exe /c echo B")
{
    DependsOn = TaskDependencies.OnId("A")
},
```

## <a name="code-sample"></a><span data-ttu-id="1d657-194">Ukázka kódu</span><span class="sxs-lookup"><span data-stu-id="1d657-194">Code sample</span></span>
<span data-ttu-id="1d657-195">[TaskDependencies] [ github_taskdependencies] ukázkový projekt je jedním z [ukázky kódu Azure Batch] [ github_samples] na Githubu.</span><span class="sxs-lookup"><span data-stu-id="1d657-195">The [TaskDependencies][github_taskdependencies] sample project is one of the [Azure Batch code samples][github_samples] on GitHub.</span></span> <span data-ttu-id="1d657-196">Toto řešení sady Visual Studio ukazuje:</span><span class="sxs-lookup"><span data-stu-id="1d657-196">This Visual Studio solution demonstrates:</span></span>

- <span data-ttu-id="1d657-197">Postup povolení úloh závislost na úlohu</span><span class="sxs-lookup"><span data-stu-id="1d657-197">How to enable task dependency on a job</span></span>
- <span data-ttu-id="1d657-198">Postup vytvoření úlohy, které závisí na jiné úlohy</span><span class="sxs-lookup"><span data-stu-id="1d657-198">How to create tasks that depend on other tasks</span></span>
- <span data-ttu-id="1d657-199">Jak provést tyto úlohy ve fondu výpočetních uzlů.</span><span class="sxs-lookup"><span data-stu-id="1d657-199">How to execute those tasks on a pool of compute nodes.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1d657-200">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1d657-200">Next steps</span></span>
### <a name="application-deployment"></a><span data-ttu-id="1d657-201">Nasazení aplikace</span><span class="sxs-lookup"><span data-stu-id="1d657-201">Application deployment</span></span>
<span data-ttu-id="1d657-202">[Balíčky aplikací](batch-application-packages.md) funkce služby Batch poskytuje snadný způsob pro obě nasazení a verze aplikace, které vaše úkoly spouští na výpočetních uzlech.</span><span class="sxs-lookup"><span data-stu-id="1d657-202">The [application packages](batch-application-packages.md) feature of Batch provides an easy way to both deploy and version the applications that your tasks execute on compute nodes.</span></span>

### <a name="installing-applications-and-staging-data"></a><span data-ttu-id="1d657-203">Instalace aplikací a pracovní data</span><span class="sxs-lookup"><span data-stu-id="1d657-203">Installing applications and staging data</span></span>
<span data-ttu-id="1d657-204">V tématu [instalaci aplikací a dat na Batch pracovních výpočetní uzly] [ forum_post] ve fóru Azure Batch přehled při přípravě uzlů ke spouštění úloh.</span><span class="sxs-lookup"><span data-stu-id="1d657-204">See [Installing applications and staging data on Batch compute nodes][forum_post] in the Azure Batch forum for an overview of methods for preparing your nodes to run tasks.</span></span> <span data-ttu-id="1d657-205">Tento příspěvek zapsána pomocí některého z členů týmu Azure Batch, je dobré Úvod do na různé způsoby, jak zkopírovat aplikace, úlohy vstupních dat a další soubory do výpočetních uzlů.</span><span class="sxs-lookup"><span data-stu-id="1d657-205">Written by one of the Azure Batch team members, this post is a good primer on the different ways to copy applications, task input data, and other files to your compute nodes.</span></span>

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

<span data-ttu-id="1d657-206">[1]: ./media/batch-task-dependency/01_one_to_one.png "Diagram: závislost 1: 1"</span><span class="sxs-lookup"><span data-stu-id="1d657-206">[1]: ./media/batch-task-dependency/01_one_to_one.png "Diagram: one-to-one dependency"</span></span>
<span data-ttu-id="1d657-207">[2]: ./media/batch-task-dependency/02_one_to_many.png "Diagram: závislost na více"</span><span class="sxs-lookup"><span data-stu-id="1d657-207">[2]: ./media/batch-task-dependency/02_one_to_many.png "Diagram: one-to-many dependency"</span></span>
<span data-ttu-id="1d657-208">[3]: ./media/batch-task-dependency/03_task_id_range.png "Diagram: Úloha id rozsah závislostí"</span><span class="sxs-lookup"><span data-stu-id="1d657-208">[3]: ./media/batch-task-dependency/03_task_id_range.png "Diagram: task id range dependency"</span></span>
