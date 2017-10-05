---
title: "Použití úkolů s více instancemi ke spouštění aplikací MPI - Azure Batch | Microsoft Docs"
description: "Zjistěte, jak ke spouštění aplikací rozhraní MPI (Message Passing) pomocí typu úlohy s více instancemi v Azure Batch."
services: batch
documentationcenter: .net
author: tamram
manager: timlt
editor: 
ms.assetid: 83e34bd7-a027-4b1b-8314-759384719327
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: 5/22/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 77d12d6d48b22dfb3e7f09f273dffc11401bb15f
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="use-multi-instance-tasks-to-run-message-passing-interface-mpi-applications-in-batch"></a><span data-ttu-id="5417e-103">Pomocí úkolů s více instancemi ke spouštění aplikací rozhraní MPI (Message Passing) ve službě Batch</span><span class="sxs-lookup"><span data-stu-id="5417e-103">Use multi-instance tasks to run Message Passing Interface (MPI) applications in Batch</span></span>

<span data-ttu-id="5417e-104">Úkoly s více instancemi umožňují spustit úlohu Azure Batch na několika výpočetních uzlech současně.</span><span class="sxs-lookup"><span data-stu-id="5417e-104">Multi-instance tasks allow you to run an Azure Batch task on multiple compute nodes simultaneously.</span></span> <span data-ttu-id="5417e-105">Tyto úlohy povolit scénáře jako aplikací rozhraní MPI (Message Passing) v dávce s vysokým výkonem.</span><span class="sxs-lookup"><span data-stu-id="5417e-105">These tasks enable high performance computing scenarios like Message Passing Interface (MPI) applications in Batch.</span></span> <span data-ttu-id="5417e-106">V tomto článku, zjistíte, jak provést úkoly s více instancemi pomocí [Batch .NET] [ api_net] knihovny.</span><span class="sxs-lookup"><span data-stu-id="5417e-106">In this article, you learn how to execute multi-instance tasks using the [Batch .NET][api_net] library.</span></span>

> [!NOTE]
> <span data-ttu-id="5417e-107">Příklady v tomto článku se zaměřují na Batch .NET, MS-MPI, a Windows výpočetní uzly, zde popsané koncepty úkolů s více instancemi platí pro další platformy a technologie (Python a Intel MPI na uzlech Linux, např.).</span><span class="sxs-lookup"><span data-stu-id="5417e-107">While the examples in this article focus on Batch .NET, MS-MPI, and Windows compute nodes, the multi-instance task concepts discussed here are applicable to other platforms and technologies (Python and Intel MPI on Linux nodes, for example).</span></span>
>
>

## <a name="multi-instance-task-overview"></a><span data-ttu-id="5417e-108">Přehled úloh s více instancemi</span><span class="sxs-lookup"><span data-stu-id="5417e-108">Multi-instance task overview</span></span>
<span data-ttu-id="5417e-109">Ve službě Batch, je obvykle každý úkol spustit na jednom výpočetním uzlu – odeslat více úkolů do úlohy, a služba Batch plánuje každý úkol pro spuštění na uzlu.</span><span class="sxs-lookup"><span data-stu-id="5417e-109">In Batch, each task is normally executed on a single compute node--you submit multiple tasks to a job, and the Batch service schedules each task for execution on a node.</span></span> <span data-ttu-id="5417e-110">Ale nakonfigurováním úkolu **s více instancemi nastavení**, řekněte dávky na místo toho vytvořte jednu primární úlohu a několik dílčích úkolů, které jsou poté provedeny ve více uzlech.</span><span class="sxs-lookup"><span data-stu-id="5417e-110">However, by configuring a task's **multi-instance settings**, you tell Batch to instead create one primary task and several subtasks that are then executed on multiple nodes.</span></span>

<span data-ttu-id="5417e-111">![Přehled úloh s více instancemi][1]</span><span class="sxs-lookup"><span data-stu-id="5417e-111">![Multi-instance task overview][1]</span></span>

<span data-ttu-id="5417e-112">Při odesílání úlohy s více instancemi nastavení do úlohy, Batch provede několik kroků jedinečný úkoly s více instancemi:</span><span class="sxs-lookup"><span data-stu-id="5417e-112">When you submit a task with multi-instance settings to a job, Batch performs several steps unique to multi-instance tasks:</span></span>

1. <span data-ttu-id="5417e-113">Služba Batch vytvoří jednu **primární** a několik **dílčí úkoly** na základě nastavení s více instancemi.</span><span class="sxs-lookup"><span data-stu-id="5417e-113">The Batch service creates one **primary** and several **subtasks** based on the multi-instance settings.</span></span> <span data-ttu-id="5417e-114">Celkový počet úloh (primární plus všechny dílčí úkoly) odpovídá počtu **instance** (výpočetní uzly) určíte v nastavení s více instancemi.</span><span class="sxs-lookup"><span data-stu-id="5417e-114">The total number of tasks (primary plus all subtasks) matches the number of **instances** (compute nodes) you specify in the multi-instance settings.</span></span>
2. <span data-ttu-id="5417e-115">Batch označí jeden výpočetních uzlů, jako **hlavní**a plány primární úlohu provést na hlavním serveru.</span><span class="sxs-lookup"><span data-stu-id="5417e-115">Batch designates one of the compute nodes as the **master**, and schedules the primary task to execute on the master.</span></span> <span data-ttu-id="5417e-116">Naplánuje dílčí úkoly pro spuštění ve zbývající části výpočetních uzlů přidělených úkol s více instancemi, jeden dílčí úlohy na uzlu.</span><span class="sxs-lookup"><span data-stu-id="5417e-116">It schedules the subtasks to execute on the remainder of the compute nodes allocated to the multi-instance task, one subtask per node.</span></span>
3. <span data-ttu-id="5417e-117">Primární a všechny dílčí úkoly stahovat **společné soubory prostředků** určíte v nastavení s více instancemi.</span><span class="sxs-lookup"><span data-stu-id="5417e-117">The primary and all subtasks download any **common resource files** you specify in the multi-instance settings.</span></span>
4. <span data-ttu-id="5417e-118">Po běžných prostředků soubory byly staženy, primární a dílčí úkoly spouštět **koordinaci příkaz** určíte v nastavení s více instancemi.</span><span class="sxs-lookup"><span data-stu-id="5417e-118">After the common resource files have been downloaded, the primary and subtasks execute the **coordination command** you specify in the multi-instance settings.</span></span> <span data-ttu-id="5417e-119">Příkaz koordinaci se obvykle používá k přípravě uzlů pro provádění úlohy.</span><span class="sxs-lookup"><span data-stu-id="5417e-119">The coordination command is typically used to prepare nodes for executing the task.</span></span> <span data-ttu-id="5417e-120">To může zahrnovat spouštění služby na pozadí (například [Microsoft MPI][msmpi_msdn]na `smpd.exe`) a ověření, že uzly jsou připravené ke zpracování zpráv mezi uzly.</span><span class="sxs-lookup"><span data-stu-id="5417e-120">This can include starting background services (such as [Microsoft MPI][msmpi_msdn]'s `smpd.exe`) and verifying that the nodes are ready to process inter-node messages.</span></span>
5. <span data-ttu-id="5417e-121">Primární úloh provede **příkaz aplikace** na hlavní uzel *po* příkaz koordinaci byla úspěšně dokončena, tak, že primární a všechny dílčí úkoly.</span><span class="sxs-lookup"><span data-stu-id="5417e-121">The primary task executes the **application command** on the master node *after* the coordination command has been completed successfully by the primary and all subtasks.</span></span> <span data-ttu-id="5417e-122">Příkaz aplikace je příkazový řádek samotný úkol s více instancemi a je proveden pouze primární úlohou.</span><span class="sxs-lookup"><span data-stu-id="5417e-122">The application command is the command line of the multi-instance task itself, and is executed only by the primary task.</span></span> <span data-ttu-id="5417e-123">V [MS-MPI][msmpi_msdn]-řešení, to je, kde můžete spuštění vaší aplikace s povolenými MPI pomocí `mpiexec.exe`.</span><span class="sxs-lookup"><span data-stu-id="5417e-123">In an [MS-MPI][msmpi_msdn]-based solution, this is where you execute your MPI-enabled application using `mpiexec.exe`.</span></span>

> [!NOTE]
> <span data-ttu-id="5417e-124">Když je funkčně distinct, "s více instancemi úloha" není jedinečný úloh typu like [StartTask] [ net_starttask] nebo [JobPreparationTask] [ net_jobprep].</span><span class="sxs-lookup"><span data-stu-id="5417e-124">Though it is functionally distinct, the "multi-instance task" is not a unique task type like the [StartTask][net_starttask] or [JobPreparationTask][net_jobprep].</span></span> <span data-ttu-id="5417e-125">Úkol s více instancemi je jednoduše standardní úlohy Batch ([CloudTask] [ net_task] v Batch .NET) byla nakonfigurována nastavení s více instancemi.</span><span class="sxs-lookup"><span data-stu-id="5417e-125">The multi-instance task is simply a standard Batch task ([CloudTask][net_task] in Batch .NET) whose multi-instance settings have been configured.</span></span> <span data-ttu-id="5417e-126">V tomto článku, označujeme jako **úkol s více instancemi**.</span><span class="sxs-lookup"><span data-stu-id="5417e-126">In this article, we refer to this as the **multi-instance task**.</span></span>
>
>

## <a name="requirements-for-multi-instance-tasks"></a><span data-ttu-id="5417e-127">Požadavky pro úkoly s více instancemi</span><span class="sxs-lookup"><span data-stu-id="5417e-127">Requirements for multi-instance tasks</span></span>
<span data-ttu-id="5417e-128">Úkoly s více instancemi vyžadují fond s **komunikaci mezi uzly povolena**a s **provedení souběžné úlohy zakázáno**.</span><span class="sxs-lookup"><span data-stu-id="5417e-128">Multi-instance tasks require a pool with **inter-node communication enabled**, and with **concurrent task execution disabled**.</span></span> <span data-ttu-id="5417e-129">Chcete-li zakázat provádění souběžných úkolů, nastavte [CloudPool.MaxTasksPerComputeNode](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool#Microsoft_Azure_Batch_CloudPool_MaxTasksPerComputeNode) vlastnost na hodnotu 1.</span><span class="sxs-lookup"><span data-stu-id="5417e-129">To disable concurrent task execution, set the [CloudPool.MaxTasksPerComputeNode](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool#Microsoft_Azure_Batch_CloudPool_MaxTasksPerComputeNode) property to 1.</span></span>

<span data-ttu-id="5417e-130">Tento fragment kódu ukazuje, jak vytvořit fond pro úkoly s více instancemi pomocí knihovny Batch .NET.</span><span class="sxs-lookup"><span data-stu-id="5417e-130">This code snippet shows how to create a pool for multi-instance tasks using the Batch .NET library.</span></span>

```csharp
CloudPool myCloudPool =
    myBatchClient.PoolOperations.CreatePool(
        poolId: "MultiInstanceSamplePool",
        targetDedicatedComputeNodes: 3
        virtualMachineSize: "small",
        cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "4"));

// Multi-instance tasks require inter-node communication, and those nodes
// must run only one task at a time.
myCloudPool.InterComputeNodeCommunicationEnabled = true;
myCloudPool.MaxTasksPerComputeNode = 1;
```

> [!NOTE]
> <span data-ttu-id="5417e-131">Pokud se pokusíte spustit úlohu s více instancemi ve fondu s komunikací internodium zakázána, nebo s *maxTasksPerNode* hodnotu větší než 1, naplánován nikdy – zůstane po neomezenou dobu ve stavu "aktivní".</span><span class="sxs-lookup"><span data-stu-id="5417e-131">If you try to run a multi-instance task in a pool with internode communication disabled, or with a *maxTasksPerNode* value greater than 1, the task is never scheduled--it remains indefinitely in the "active" state.</span></span> 
>
> <span data-ttu-id="5417e-132">Úkoly s více instancemi lze spustit pouze v uzlů ve fondech vytvořený po 14. prosince 2015.</span><span class="sxs-lookup"><span data-stu-id="5417e-132">Multi-instance tasks can execute only on nodes in pools created after 14 December 2015.</span></span>
>
>

### <a name="use-a-starttask-to-install-mpi"></a><span data-ttu-id="5417e-133">StartTask použít k instalaci MPI</span><span class="sxs-lookup"><span data-stu-id="5417e-133">Use a StartTask to install MPI</span></span>
<span data-ttu-id="5417e-134">Ke spouštění aplikací MPI s více instancemi úloh, musíte nejprve nainstalovat implementace MPI (MS-MPI nebo MPI Intel, např.) na výpočetních uzlech ve fondu.</span><span class="sxs-lookup"><span data-stu-id="5417e-134">To run MPI applications with a multi-instance task, you first need to install an MPI implementation (MS-MPI or Intel MPI, for example) on the compute nodes in the pool.</span></span> <span data-ttu-id="5417e-135">Toto je vhodná doba na použití [StartTask][net_starttask], který provede vždy, když se uzel připojí fondu nebo je restartovat.</span><span class="sxs-lookup"><span data-stu-id="5417e-135">This is a good time to use a [StartTask][net_starttask], which executes whenever a node joins a pool, or is restarted.</span></span> <span data-ttu-id="5417e-136">Tento fragment kódu vytvoří StartTask, která určuje instalační balíček MS-MPI jako [souboru prostředků][net_resourcefile].</span><span class="sxs-lookup"><span data-stu-id="5417e-136">This code snippet creates a StartTask that specifies the MS-MPI setup package as a [resource file][net_resourcefile].</span></span> <span data-ttu-id="5417e-137">Spouštěcí úkol příkazový řádek se spustí po souboru prostředků se stáhne do uzlu.</span><span class="sxs-lookup"><span data-stu-id="5417e-137">The start task's command line is executed after the resource file is downloaded to the node.</span></span> <span data-ttu-id="5417e-138">V takovém případě příkazový řádek provede bezobslužnou instalaci MS MPI.</span><span class="sxs-lookup"><span data-stu-id="5417e-138">In this case, the command line performs an unattended install of MS-MPI.</span></span>

```csharp
// Create a StartTask for the pool which we use for installing MS-MPI on
// the nodes as they join the pool (or when they are restarted).
StartTask startTask = new StartTask
{
    CommandLine = "cmd /c MSMpiSetup.exe -unattend -force",
    ResourceFiles = new List<ResourceFile> { new ResourceFile("https://mystorageaccount.blob.core.windows.net/mycontainer/MSMpiSetup.exe", "MSMpiSetup.exe") },
    UserIdentity = new UserIdentity(new AutoUserSpecification(elevationLevel: ElevationLevel.Admin)),
    WaitForSuccess = true
};
myCloudPool.StartTask = startTask;

// Commit the fully configured pool to the Batch service to actually create
// the pool and its compute nodes.
await myCloudPool.CommitAsync();
```

### <a name="remote-direct-memory-access-rdma"></a><span data-ttu-id="5417e-139">Vzdálený přímý přístup do paměti (vzdáleného počítače RDMA)</span><span class="sxs-lookup"><span data-stu-id="5417e-139">Remote direct memory access (RDMA)</span></span>
<span data-ttu-id="5417e-140">Pokud vyberete [RDMA podporovat velikost](../virtual-machines/windows/sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) například A9 pro výpočetní uzly ve fondu Batch, MPI aplikace mohou využít výhod Azure vysoce výkonné, nízkou latencí paměti vzdálený přímý přístup do (počítače RDMA) sítě.</span><span class="sxs-lookup"><span data-stu-id="5417e-140">When you choose an [RDMA-capable size](../virtual-machines/windows/sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) such as A9 for the compute nodes in your Batch pool, your MPI application can take advantage of Azure's high-performance, low-latency remote direct memory access (RDMA) network.</span></span>

<span data-ttu-id="5417e-141">Vyhledejte velikosti definované jako "Podporující RDMA" v těchto článcích:</span><span class="sxs-lookup"><span data-stu-id="5417e-141">Look for the sizes specified as "RDMA capable" in the following articles:</span></span>

* <span data-ttu-id="5417e-142">**CloudServiceConfiguration** fondy</span><span class="sxs-lookup"><span data-stu-id="5417e-142">**CloudServiceConfiguration** pools</span></span>

  * <span data-ttu-id="5417e-143">[Velikosti pro cloudové služby](../cloud-services/cloud-services-sizes-specs.md) (jenom Windows)</span><span class="sxs-lookup"><span data-stu-id="5417e-143">[Sizes for Cloud Services](../cloud-services/cloud-services-sizes-specs.md) (Windows only)</span></span>
* <span data-ttu-id="5417e-144">**VirtualMachineConfiguration** fondy</span><span class="sxs-lookup"><span data-stu-id="5417e-144">**VirtualMachineConfiguration** pools</span></span>

  * <span data-ttu-id="5417e-145">[Velikosti virtuálních počítačů v Azure](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (Linux)</span><span class="sxs-lookup"><span data-stu-id="5417e-145">[Sizes for virtual machines in Azure](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (Linux)</span></span>
  * <span data-ttu-id="5417e-146">[Velikosti virtuálních počítačů v Azure](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (Windows)</span><span class="sxs-lookup"><span data-stu-id="5417e-146">[Sizes for virtual machines in Azure](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (Windows)</span></span>

> [!NOTE]
> <span data-ttu-id="5417e-147">Abyste mohli využívat funkce RDMA [Linuxových výpočetních uzlů](batch-linux-nodes.md), je nutné použít **Intel MPI** na uzlech.</span><span class="sxs-lookup"><span data-stu-id="5417e-147">To take advantage of RDMA on [Linux compute nodes](batch-linux-nodes.md), you must use **Intel MPI** on the nodes.</span></span> <span data-ttu-id="5417e-148">Další informace o CloudServiceConfiguration a VirtualMachineConfiguration fondy, najdete v části fondu [přehled funkcí Batch](batch-api-basics.md).</span><span class="sxs-lookup"><span data-stu-id="5417e-148">For more information on CloudServiceConfiguration and VirtualMachineConfiguration pools, see the Pool section of the [Batch feature overview](batch-api-basics.md).</span></span>
>
>

## <a name="create-a-multi-instance-task-with-batch-net"></a><span data-ttu-id="5417e-149">Vytvoření úlohy s více instancemi pomocí rozhraní Batch .NET</span><span class="sxs-lookup"><span data-stu-id="5417e-149">Create a multi-instance task with Batch .NET</span></span>
<span data-ttu-id="5417e-150">Teď, když jsme si zahrnutých fondu požadavky a instalace balíčku MPI, vytvoříme úlohu s více instancemi.</span><span class="sxs-lookup"><span data-stu-id="5417e-150">Now that we've covered the pool requirements and MPI package installation, let's create the multi-instance task.</span></span> <span data-ttu-id="5417e-151">V tento fragment kódu, vytvoříme standard [CloudTask][net_task], nakonfigurujte její [MultiInstanceSettings] [ net_multiinstance_prop] vlastnost.</span><span class="sxs-lookup"><span data-stu-id="5417e-151">In this snippet, we create a standard [CloudTask][net_task], then configure its [MultiInstanceSettings][net_multiinstance_prop] property.</span></span> <span data-ttu-id="5417e-152">Jak už bylo zmíněno dříve, není úkol s více instancemi typu distinct úloh, ale standardní úlohy Batch nakonfigurované s nastavením s více instancemi.</span><span class="sxs-lookup"><span data-stu-id="5417e-152">As mentioned earlier, the multi-instance task is not a distinct task type, but a standard Batch task configured with multi-instance settings.</span></span>

```csharp
// Create the multi-instance task. Its command line is the "application command"
// and will be executed *only* by the primary, and only after the primary and
// subtasks execute the CoordinationCommandLine.
CloudTask myMultiInstanceTask = new CloudTask(id: "mymultiinstancetask",
    commandline: "cmd /c mpiexec.exe -wdir %AZ_BATCH_TASK_SHARED_DIR% MyMPIApplication.exe");

// Configure the task's MultiInstanceSettings. The CoordinationCommandLine will be executed by
// the primary and all subtasks.
myMultiInstanceTask.MultiInstanceSettings =
    new MultiInstanceSettings(numberOfNodes) {
    CoordinationCommandLine = @"cmd /c start cmd /c ""%MSMPI_BIN%\smpd.exe"" -d",
    CommonResourceFiles = new List<ResourceFile> {
    new ResourceFile("https://mystorageaccount.blob.core.windows.net/mycontainer/MyMPIApplication.exe",
                     "MyMPIApplication.exe")
    }
};

// Submit the task to the job. Batch will take care of splitting it into subtasks and
// scheduling them for execution on the nodes.
await myBatchClient.JobOperations.AddTaskAsync("mybatchjob", myMultiInstanceTask);
```

## <a name="primary-task-and-subtasks"></a><span data-ttu-id="5417e-153">Primární úlohy a dílčí úkoly</span><span class="sxs-lookup"><span data-stu-id="5417e-153">Primary task and subtasks</span></span>
<span data-ttu-id="5417e-154">Když vytvoříte nastavení s více instancemi pro úlohu, je třeba zadat počet výpočetních uzlů, které jsou při provádění úlohy.</span><span class="sxs-lookup"><span data-stu-id="5417e-154">When you create the multi-instance settings for a task, you specify the number of compute nodes that are to execute the task.</span></span> <span data-ttu-id="5417e-155">Při odesílání úloh do úlohy, služba Batch vytvoří jednu **primární** úlohy a dostatek **dílčí úkoly** společně odpovídající počet uzlů, které jste zadali.</span><span class="sxs-lookup"><span data-stu-id="5417e-155">When you submit the task to a job, the Batch service creates one **primary** task and enough **subtasks** that together match the number of nodes you specified.</span></span>

<span data-ttu-id="5417e-156">Tyto úlohy jsou přiřazeny id celé číslo v rozsahu od 0 do *numberOfInstances* - 1.</span><span class="sxs-lookup"><span data-stu-id="5417e-156">These tasks are assigned an integer id in the range of 0 to *numberOfInstances* - 1.</span></span> <span data-ttu-id="5417e-157">Úlohu s id 0 je primární úlohy a všechny ostatní ID jsou dílčí úkoly.</span><span class="sxs-lookup"><span data-stu-id="5417e-157">The task with id 0 is the primary task, and all other ids are subtasks.</span></span> <span data-ttu-id="5417e-158">Například pokud vytvoříte následující nastavení s více instancemi pro úlohu, primární úlohy má id 0 a dílčí úkoly by měla mít ID 1 až 9.</span><span class="sxs-lookup"><span data-stu-id="5417e-158">For example, if you create the following multi-instance settings for a task, the primary task would have an id of 0, and the subtasks would have ids 1 through 9.</span></span>

```csharp
int numberOfNodes = 10;
myMultiInstanceTask.MultiInstanceSettings = new MultiInstanceSettings(numberOfNodes);
```

### <a name="master-node"></a><span data-ttu-id="5417e-159">Hlavní uzel</span><span class="sxs-lookup"><span data-stu-id="5417e-159">Master node</span></span>
<span data-ttu-id="5417e-160">Při odesílání úlohy s více instancemi služby Batch označí jeden výpočetní uzly jako "hlavní" uzel a primární úloh spustit na hlavní uzel.</span><span class="sxs-lookup"><span data-stu-id="5417e-160">When you submit a multi-instance task, the Batch service designates one of the compute nodes as the "master" node, and schedules the primary task to execute on the master node.</span></span> <span data-ttu-id="5417e-161">Dílčí úkoly jsou naplánovány pro spuštění ve zbývající části uzlů přidělených pro úlohu s více instancemi.</span><span class="sxs-lookup"><span data-stu-id="5417e-161">The subtasks are scheduled to execute on the remainder of the nodes allocated to the multi-instance task.</span></span>

## <a name="coordination-command"></a><span data-ttu-id="5417e-162">Příkaz spolupráce</span><span class="sxs-lookup"><span data-stu-id="5417e-162">Coordination command</span></span>
<span data-ttu-id="5417e-163">**Koordinaci příkaz** je provedený obě primární a dílčích úkolů.</span><span class="sxs-lookup"><span data-stu-id="5417e-163">The **coordination command** is executed by both the primary and subtasks.</span></span>

<span data-ttu-id="5417e-164">Vyvolání příkazu koordinaci blokuje – Batch příkaz aplikace nespustí, dokud úspěšně vrátila příkaz spolupráce pro všechny dílčí úkoly.</span><span class="sxs-lookup"><span data-stu-id="5417e-164">The invocation of the coordination command is blocking--Batch does not execute the application command until the coordination command has returned successfully for all subtasks.</span></span> <span data-ttu-id="5417e-165">Příkaz koordinaci by proto spustit žádné služby, požadované pozadí, ověřte, zda je připravený k použití a poté ukončete.</span><span class="sxs-lookup"><span data-stu-id="5417e-165">The coordination command should therefore start any required background services, verify that they are ready for use, and then exit.</span></span> <span data-ttu-id="5417e-166">Například tento příkaz koordinaci pro řešení prostřednictvím MS-MPI verze 7 spustí službu SMPD na uzlu, pak bude ukončen:</span><span class="sxs-lookup"><span data-stu-id="5417e-166">For example, this coordination command for a solution using MS-MPI version 7 starts the SMPD service on the node, then exits:</span></span>

```
cmd /c start cmd /c ""%MSMPI_BIN%\smpd.exe"" -d
```

<span data-ttu-id="5417e-167">Všimněte si použití `start` v tomto příkazu spolupráce.</span><span class="sxs-lookup"><span data-stu-id="5417e-167">Note the use of `start` in this coordination command.</span></span> <span data-ttu-id="5417e-168">Toto je nutné kvůli `smpd.exe` aplikace nevrátí okamžitě po spuštění.</span><span class="sxs-lookup"><span data-stu-id="5417e-168">This is required because the `smpd.exe` application does not return immediately after execution.</span></span> <span data-ttu-id="5417e-169">Bez použití [spustit] [ cmd_start] příkaz, tento příkaz koordinaci by vrátit a by proto blokovat příkaz aplikace spuštění.</span><span class="sxs-lookup"><span data-stu-id="5417e-169">Without the use of the [start][cmd_start] command, this coordination command would not return, and would therefore block the application command from running.</span></span>

## <a name="application-command"></a><span data-ttu-id="5417e-170">Příkaz aplikace</span><span class="sxs-lookup"><span data-stu-id="5417e-170">Application command</span></span>
<span data-ttu-id="5417e-171">Po primární úlohy a všechny dílčí úkoly dokončení provádění příkazu koordinaci, úkolů s více instancemi příkazový řádek se spustí primární úlohou *pouze*.</span><span class="sxs-lookup"><span data-stu-id="5417e-171">Once the primary task and all subtasks have finished executing the coordination command, the multi-instance task's command line is executed by the primary task *only*.</span></span> <span data-ttu-id="5417e-172">To říkáme **příkaz aplikace** ho odlišuje od příkaz spolupráce.</span><span class="sxs-lookup"><span data-stu-id="5417e-172">We call this the **application command** to distinguish it from the coordination command.</span></span>

<span data-ttu-id="5417e-173">U aplikací, MS-MPI, použijte příkaz aplikaci k provedení vaší aplikace s povolenými MPI s `mpiexec.exe`.</span><span class="sxs-lookup"><span data-stu-id="5417e-173">For MS-MPI applications, use the application command to execute your MPI-enabled application with `mpiexec.exe`.</span></span> <span data-ttu-id="5417e-174">Zde je ukázka, příkaz aplikace pro řešení pomocí MS-MPI verze 7:</span><span class="sxs-lookup"><span data-stu-id="5417e-174">For example, here is an application command for a solution using MS-MPI version 7:</span></span>

```
cmd /c ""%MSMPI_BIN%\mpiexec.exe"" -c 1 -wdir %AZ_BATCH_TASK_SHARED_DIR% MyMPIApplication.exe
```

> [!NOTE]
> <span data-ttu-id="5417e-175">Protože MS-MPI `mpiexec.exe` používá `CCP_NODES` proměnné ve výchozím nastavení (najdete v části [proměnné prostředí](#environment-variables)) příkazového řádku aplikace příkladu výše vyloučí ho.</span><span class="sxs-lookup"><span data-stu-id="5417e-175">Because MS-MPI's `mpiexec.exe` uses the `CCP_NODES` variable by default (see [Environment variables](#environment-variables)) the example application command line above excludes it.</span></span>
>
>

## <a name="environment-variables"></a><span data-ttu-id="5417e-176">Proměnné prostředí</span><span class="sxs-lookup"><span data-stu-id="5417e-176">Environment variables</span></span>
<span data-ttu-id="5417e-177">Batch vytvoří několik [proměnné prostředí] [ msdn_env_var] specifické pro úkoly na výpočetních uzlů přidělených úkolů s více instancemi s více instancemi.</span><span class="sxs-lookup"><span data-stu-id="5417e-177">Batch creates several [environment variables][msdn_env_var] specific to multi-instance tasks on the compute nodes allocated to a multi-instance task.</span></span> <span data-ttu-id="5417e-178">Příkazové řádky koordinace a aplikace můžete odkazovat těchto proměnných prostředí, jak můžete skripty a programů, které se provést.</span><span class="sxs-lookup"><span data-stu-id="5417e-178">Your coordination and application command lines can reference these environment variables, as can the scripts and programs they execute.</span></span>

<span data-ttu-id="5417e-179">Následující proměnné prostředí jsou vytvořeny pomocí služby Batch pro použití s více instancemi úlohy:</span><span class="sxs-lookup"><span data-stu-id="5417e-179">The following environment variables are created by the Batch service for use by multi-instance tasks:</span></span>

* `CCP_NODES`
* `AZ_BATCH_NODE_LIST`
* `AZ_BATCH_HOST_LIST`
* `AZ_BATCH_MASTER_NODE`
* `AZ_BATCH_TASK_SHARED_DIR`
* `AZ_BATCH_IS_CURRENT_NODE_MASTER`

<span data-ttu-id="5417e-180">Úplné podrobnosti o těchto a dalších dávky výpočetní uzel proměnné prostředí, včetně jejich obsah a viditelnost, najdete v části [výpočetní uzel proměnné prostředí][msdn_env_var].</span><span class="sxs-lookup"><span data-stu-id="5417e-180">For full details on these and the other Batch compute node environment variables, including their contents and visibility, see [Compute node environment variables][msdn_env_var].</span></span>

> [!TIP]
> <span data-ttu-id="5417e-181">Ukázka kódu Batch Linux MPI obsahuje příklad některé z těchto proměnných prostředí použití.</span><span class="sxs-lookup"><span data-stu-id="5417e-181">The Batch Linux MPI code sample contains an example of how several of these environment variables can be used.</span></span> <span data-ttu-id="5417e-182">[Koordinaci cmd] [ coord_cmd_example] Bash skript stáhne běžné aplikace a vstupní soubory ze služby Azure Storage, umožňuje sdílené složky systému souborů NFS v hlavní uzel a nakonfiguruje ostatní uzly jako klienti NFS přidělit úkol s více instancemi.</span><span class="sxs-lookup"><span data-stu-id="5417e-182">The [coordination-cmd][coord_cmd_example] Bash script downloads common application and input files from Azure Storage, enables a Network File System (NFS) share on the master node, and configures the other nodes allocated to the multi-instance task as NFS clients.</span></span>
>
>

## <a name="resource-files"></a><span data-ttu-id="5417e-183">Soubory prostředků</span><span class="sxs-lookup"><span data-stu-id="5417e-183">Resource files</span></span>
<span data-ttu-id="5417e-184">Existují dvě sady souborů prostředků vzít v úvahu pro úkoly s více instancemi: **společné soubory prostředků** , *všechny* úkoly stahují (obě primární a dílčí úkoly) a **soubory prostředků** zadaná pro více instancemi úkolů sebe, což *pouze primární* úkolů stahování.</span><span class="sxs-lookup"><span data-stu-id="5417e-184">There are two sets of resource files to consider for multi-instance tasks: **common resource files** that *all* tasks download (both primary and subtasks), and the **resource files** specified for the multi-instance task itself, which *only the primary* task downloads.</span></span>

<span data-ttu-id="5417e-185">Můžete určit jeden nebo více **společné soubory prostředků** v nastavení s více instancemi pro úlohu.</span><span class="sxs-lookup"><span data-stu-id="5417e-185">You can specify one or more **common resource files** in the multi-instance settings for a task.</span></span> <span data-ttu-id="5417e-186">Tyto společné soubory prostředků se stahují z [Azure Storage](../storage/common/storage-introduction.md) do každého uzlu **sdíleného adresáře úkolu** primární a všechny dílčí úkoly.</span><span class="sxs-lookup"><span data-stu-id="5417e-186">These common resource files are downloaded from [Azure Storage](../storage/common/storage-introduction.md) into each node's **task shared directory** by the primary and all subtasks.</span></span> <span data-ttu-id="5417e-187">Do sdíleného adresáře úkolu můžete přístup z aplikace a koordinaci příkazové řádky pomocí `AZ_BATCH_TASK_SHARED_DIR` proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="5417e-187">You can access the task shared directory from application and coordination command lines by using the `AZ_BATCH_TASK_SHARED_DIR` environment variable.</span></span> <span data-ttu-id="5417e-188">`AZ_BATCH_TASK_SHARED_DIR` Cesta je stejné pro všechny uzly přidělit úkol s více instancemi, proto můžete sdílet jeden koordinaci příkaz mezi primárním serverem a všechny dílčí úkoly.</span><span class="sxs-lookup"><span data-stu-id="5417e-188">The `AZ_BATCH_TASK_SHARED_DIR` path is identical on every node allocated to the multi-instance task, thus you can share a single coordination command between the primary and all subtasks.</span></span> <span data-ttu-id="5417e-189">Batch "nesdílí" v adresáři v tom smyslu vzdáleného přístupu, ale můžete ho použít jako přípojný nebo sdílení bodu, jak je uvedeno výše v tip na proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="5417e-189">Batch does not "share" the directory in a remote access sense, but you can use it as a mount or share point as mentioned earlier in the tip on environment variables.</span></span>

<span data-ttu-id="5417e-190">Soubory prostředků, které zadáte pro samotný úkol s více instancemi se stáhnou do pracovního adresáře úkolu, `AZ_BATCH_TASK_WORKING_DIR`, ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="5417e-190">Resource files that you specify for the multi-instance task itself are downloaded to the task's working directory, `AZ_BATCH_TASK_WORKING_DIR`, by default.</span></span> <span data-ttu-id="5417e-191">Jak je uvedeno, na rozdíl od běžných soubory prostředků, pouze primární úkol stáhne soubory prostředků, které jsou zadané pro úlohu s více instancemi sám sebe.</span><span class="sxs-lookup"><span data-stu-id="5417e-191">As mentioned, in contrast to common resource files, only the primary task downloads resource files specified for the  multi-instance task itself.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="5417e-192">Vždy použít proměnné prostředí `AZ_BATCH_TASK_SHARED_DIR` a `AZ_BATCH_TASK_WORKING_DIR` odkazovat na tyto adresáře v příkazové řádky.</span><span class="sxs-lookup"><span data-stu-id="5417e-192">Always use the environment variables `AZ_BATCH_TASK_SHARED_DIR` and `AZ_BATCH_TASK_WORKING_DIR` to refer to these directories in your command lines.</span></span> <span data-ttu-id="5417e-193">Nepokoušejte se ručně vytvořit cesty.</span><span class="sxs-lookup"><span data-stu-id="5417e-193">Do not attempt to construct the paths manually.</span></span>
>
>

## <a name="task-lifetime"></a><span data-ttu-id="5417e-194">Doba platnosti úloh</span><span class="sxs-lookup"><span data-stu-id="5417e-194">Task lifetime</span></span>
<span data-ttu-id="5417e-195">Doba platnosti primární úlohy řídí životnost úlohu celý s více instancemi.</span><span class="sxs-lookup"><span data-stu-id="5417e-195">The lifetime of the primary task controls the lifetime of the entire multi-instance task.</span></span> <span data-ttu-id="5417e-196">Při ukončení primární, budou ukončeny všechny jeho dílčí úkoly.</span><span class="sxs-lookup"><span data-stu-id="5417e-196">When the primary exits, all of the subtasks are terminated.</span></span> <span data-ttu-id="5417e-197">Ukončovací kód primární kód ukončení úlohy a proto slouží k určení úspěch nebo selhání úlohy pro účely opakování.</span><span class="sxs-lookup"><span data-stu-id="5417e-197">The exit code of the primary is the exit code of the task, and is therefore used to determine the success or failure of the task for retry purposes.</span></span>

<span data-ttu-id="5417e-198">Pokud žádný z dílčích úkolů selže, ukončení s návratovým kódem nulová, například celou s více instancemi úloha nezdaří.</span><span class="sxs-lookup"><span data-stu-id="5417e-198">If any of the subtasks fail, exiting with a non-zero return code, for example, the entire multi-instance task fails.</span></span> <span data-ttu-id="5417e-199">Úkol s více instancemi je pak byla ukončena a opakovat, až do limitu opakování.</span><span class="sxs-lookup"><span data-stu-id="5417e-199">The multi-instance task is then terminated and retried, up to its retry limit.</span></span>

<span data-ttu-id="5417e-200">Při odstranění úlohu s více instancemi primárním serverem a všechny dílčí úkoly jsou odstraněny také službou Batch.</span><span class="sxs-lookup"><span data-stu-id="5417e-200">When you delete a multi-instance task, the primary and all subtasks are also deleted by the Batch service.</span></span> <span data-ttu-id="5417e-201">Všechny dílčí úloha adresáře a jejich soubory se odstraní z výpočetních uzlů, stejně jako u standardní úlohy.</span><span class="sxs-lookup"><span data-stu-id="5417e-201">All subtask directories and their files are deleted from the compute nodes, just as for a standard task.</span></span>

<span data-ttu-id="5417e-202">[TaskConstraints] [ net_taskconstraints] pro úlohu s více instancemi, jako [MaxTaskRetryCount][net_taskconstraint_maxretry], [MaxWallClockTime] [ net_taskconstraint_maxwallclock], a [RetentionTime] [ net_taskconstraint_retention] vlastnosti, se neuplatňují, jak jsou pro standardní úlohy a použít na primárním a všechny dílčí úkoly.</span><span class="sxs-lookup"><span data-stu-id="5417e-202">[TaskConstraints][net_taskconstraints] for a multi-instance task, such as the [MaxTaskRetryCount][net_taskconstraint_maxretry], [MaxWallClockTime][net_taskconstraint_maxwallclock], and [RetentionTime][net_taskconstraint_retention] properties, are honored as they are for a standard task, and apply to the primary and all subtasks.</span></span> <span data-ttu-id="5417e-203">Nicméně pokud změníte [RetentionTime] [ net_taskconstraint_retention] vlastnost po přidání úkolů s více instancemi úlohou, tato změna se aplikuje jenom na primární úlohy.</span><span class="sxs-lookup"><span data-stu-id="5417e-203">However, if you change the [RetentionTime][net_taskconstraint_retention] property after adding the multi-instance task to the job, this change is applied only to the primary task.</span></span> <span data-ttu-id="5417e-204">Všechny jeho dílčí úkoly nadále používat původní [RetentionTime][net_taskconstraint_retention].</span><span class="sxs-lookup"><span data-stu-id="5417e-204">All of the subtasks continue to use the original [RetentionTime][net_taskconstraint_retention].</span></span>

<span data-ttu-id="5417e-205">Seznam posledních úkolů výpočetním uzlu odráží id dílčí úlohy, pokud poslední úloha byla součástí úlohu s více instancemi.</span><span class="sxs-lookup"><span data-stu-id="5417e-205">A compute node's recent task list reflects the id of a subtask if the recent task was part of a multi-instance task.</span></span>

## <a name="obtain-information-about-subtasks"></a><span data-ttu-id="5417e-206">Získání informací o dílčí úkoly</span><span class="sxs-lookup"><span data-stu-id="5417e-206">Obtain information about subtasks</span></span>
<span data-ttu-id="5417e-207">Chcete-li získat informace o dílčí úkoly pomocí knihovny Batch .NET, zavolejte [CloudTask.ListSubtasks] [ net_task_listsubtasks] metoda.</span><span class="sxs-lookup"><span data-stu-id="5417e-207">To obtain information on subtasks by using the Batch .NET library, call the [CloudTask.ListSubtasks][net_task_listsubtasks] method.</span></span> <span data-ttu-id="5417e-208">Tato metoda vrátí informace o všechny dílčí úkoly a informace o výpočetním uzlu, která spouští úkoly.</span><span class="sxs-lookup"><span data-stu-id="5417e-208">This method returns information on all subtasks, and information about the compute node that executed the tasks.</span></span> <span data-ttu-id="5417e-209">Z těchto informací můžete určit každý dílčí úlohy kořenový adresář, id fondu, jeho aktuální stav, ukončovacího kódu a další.</span><span class="sxs-lookup"><span data-stu-id="5417e-209">From this information, you can determine each subtask's root directory, the pool id, its current state, exit code, and more.</span></span> <span data-ttu-id="5417e-210">Tyto informace můžete použít v kombinaci s [PoolOperations.GetNodeFile] [ poolops_getnodefile] metoda získat soubory dílčí úlohy.</span><span class="sxs-lookup"><span data-stu-id="5417e-210">You can use this information in combination with the [PoolOperations.GetNodeFile][poolops_getnodefile] method to obtain the subtask's files.</span></span> <span data-ttu-id="5417e-211">Všimněte si, že tato metoda nevrátí informace pro primární úlohu (id 0).</span><span class="sxs-lookup"><span data-stu-id="5417e-211">Note that this method does not return information for the primary task (id 0).</span></span>

> [!NOTE]
> <span data-ttu-id="5417e-212">Pokud není uvedeno jinak, metody rozhraní Batch .NET, které pracují na s více instancemi [CloudTask] [ net_task] samotné použít *pouze* primární úlohy.</span><span class="sxs-lookup"><span data-stu-id="5417e-212">Unless otherwise stated, Batch .NET methods that operate on the multi-instance [CloudTask][net_task] itself apply *only* to the primary task.</span></span> <span data-ttu-id="5417e-213">Například při volání [CloudTask.ListNodeFiles] [ net_task_listnodefiles] metodu úlohu s více instancemi, jsou vráceny pouze soubory primární úkolu.</span><span class="sxs-lookup"><span data-stu-id="5417e-213">For example, when you call the [CloudTask.ListNodeFiles][net_task_listnodefiles] method on a multi-instance task, only the primary task's files are returned.</span></span>
>
>

<span data-ttu-id="5417e-214">Následující fragment kódu ukazuje, jak získat informace o dílčí úlohy a také žádosti o obsah souboru z uzlů, ve kterých provést.</span><span class="sxs-lookup"><span data-stu-id="5417e-214">The following code snippet shows how to obtain subtask information, as well as request file contents from the nodes on which they executed.</span></span>

```csharp
// Obtain the job and the multi-instance task from the Batch service
CloudJob boundJob = batchClient.JobOperations.GetJob("mybatchjob");
CloudTask myMultiInstanceTask = boundJob.GetTask("mymultiinstancetask");

// Now obtain the list of subtasks for the task
IPagedEnumerable<SubtaskInformation> subtasks = myMultiInstanceTask.ListSubtasks();

// Asynchronously iterate over the subtasks and print their stdout and stderr
// output if the subtask has completed
await subtasks.ForEachAsync(async (subtask) =>
{
    Console.WriteLine("subtask: {0}", subtask.Id);
    Console.WriteLine("exit code: {0}", subtask.ExitCode);

    if (subtask.State == SubtaskState.Completed)
    {
        ComputeNode node =
            await batchClient.PoolOperations.GetComputeNodeAsync(subtask.ComputeNodeInformation.PoolId,
                                                                 subtask.ComputeNodeInformation.ComputeNodeId);

        NodeFile stdOutFile = await node.GetNodeFileAsync(subtask.ComputeNodeInformation.TaskRootDirectory + "\\" + Constants.StandardOutFileName);
        NodeFile stdErrFile = await node.GetNodeFileAsync(subtask.ComputeNodeInformation.TaskRootDirectory + "\\" + Constants.StandardErrorFileName);
        stdOut = await stdOutFile.ReadAsStringAsync();
        stdErr = await stdErrFile.ReadAsStringAsync();

        Console.WriteLine("node: {0}:", node.Id);
        Console.WriteLine("stdout.txt: {0}", stdOut);
        Console.WriteLine("stderr.txt: {0}", stdErr);
    }
    else
    {
        Console.WriteLine("\tSubtask {0} is in state {1}", subtask.Id, subtask.State);
    }
});
```

## <a name="code-sample"></a><span data-ttu-id="5417e-215">Ukázka kódu</span><span class="sxs-lookup"><span data-stu-id="5417e-215">Code sample</span></span>
<span data-ttu-id="5417e-216">[MultiInstanceTasks] [ github_mpi] ukázka kódu na Githubu ukazuje, jak se použije ke spuštění úlohy s více instancemi [MS-MPI] [ msmpi_msdn] aplikace na Batch výpočetních uzlů.</span><span class="sxs-lookup"><span data-stu-id="5417e-216">The [MultiInstanceTasks][github_mpi] code sample on GitHub demonstrates how to use a multi-instance task to run an [MS-MPI][msmpi_msdn] application on Batch compute nodes.</span></span> <span data-ttu-id="5417e-217">Postupujte podle kroků v [přípravy](#preparation) a [provádění](#execution) ke spuštění ukázky.</span><span class="sxs-lookup"><span data-stu-id="5417e-217">Follow the steps in [Preparation](#preparation) and [Execution](#execution) to run the sample.</span></span>

### <a name="preparation"></a><span data-ttu-id="5417e-218">Příprava</span><span class="sxs-lookup"><span data-stu-id="5417e-218">Preparation</span></span>
1. <span data-ttu-id="5417e-219">První dva postupujte podle [postup zkompilování a spuštění jednoduchý program MS-MPI][msmpi_howto].</span><span class="sxs-lookup"><span data-stu-id="5417e-219">Follow the first two steps in [How to compile and run a simple MS-MPI program][msmpi_howto].</span></span> <span data-ttu-id="5417e-220">To splňuje prerequesites pro následující krok.</span><span class="sxs-lookup"><span data-stu-id="5417e-220">This satisfies the prerequesites for the following step.</span></span>
2. <span data-ttu-id="5417e-221">Sestavení *verze* verzi [MPIHelloWorld] [ helloworld_proj] ukázka MPI programu.</span><span class="sxs-lookup"><span data-stu-id="5417e-221">Build a *Release* version of the [MPIHelloWorld][helloworld_proj] sample MPI program.</span></span> <span data-ttu-id="5417e-222">Toto je program, který se spustí na výpočetních uzlech úlohou s více instancemi.</span><span class="sxs-lookup"><span data-stu-id="5417e-222">This is the program that will be run on compute nodes by the multi-instance task.</span></span>
3. <span data-ttu-id="5417e-223">Vytvořte soubor zip obsahující `MPIHelloWorld.exe` (které je vytvořené v kroku 2) a `MSMpiSetup.exe` (který jste stáhli krok 1).</span><span class="sxs-lookup"><span data-stu-id="5417e-223">Create a zip file containing `MPIHelloWorld.exe` (which you built step 2) and `MSMpiSetup.exe` (which you downloaded step 1).</span></span> <span data-ttu-id="5417e-224">Tento soubor zip budete nahrát jako balíček aplikace v dalším kroku.</span><span class="sxs-lookup"><span data-stu-id="5417e-224">You'll upload this zip file as an application package in the next step.</span></span>
4. <span data-ttu-id="5417e-225">Použití [portál Azure] [ portal] k vytvoření dávky [aplikace](batch-application-packages.md) nazývá "MPIHelloWorld" a zadejte soubor zip, který jste vytvořili v předchozím kroku jako verze "1.0" balíček aplikace.</span><span class="sxs-lookup"><span data-stu-id="5417e-225">Use the [Azure portal][portal] to create a Batch [application](batch-application-packages.md) called "MPIHelloWorld", and specify the zip file you created in the previous step as version "1.0" of the application package.</span></span> <span data-ttu-id="5417e-226">V tématu [odesílat a spravovat aplikace](batch-application-packages.md#upload-and-manage-applications) Další informace.</span><span class="sxs-lookup"><span data-stu-id="5417e-226">See [Upload and manage applications](batch-application-packages.md#upload-and-manage-applications) for more information.</span></span>

> [!TIP]
> <span data-ttu-id="5417e-227">Sestavení *verze* verzi `MPIHelloWorld.exe` tak, že nemáte žádné další závislosti patří (například `msvcp140d.dll` nebo `vcruntime140d.dll`) v balíčku aplikace.</span><span class="sxs-lookup"><span data-stu-id="5417e-227">Build a *Release* version of `MPIHelloWorld.exe` so that you don't have to include any additional dependencies (for example, `msvcp140d.dll` or `vcruntime140d.dll`) in your application package.</span></span>
>
>

### <a name="execution"></a><span data-ttu-id="5417e-228">Provádění</span><span class="sxs-lookup"><span data-stu-id="5417e-228">Execution</span></span>
1. <span data-ttu-id="5417e-229">Stažení [azure-batch-samples] [ github_samples_zip] z Githubu.</span><span class="sxs-lookup"><span data-stu-id="5417e-229">Download the [azure-batch-samples][github_samples_zip] from GitHub.</span></span>
2. <span data-ttu-id="5417e-230">Otevřete MultiInstanceTasks **řešení** v sadě Visual Studio 2015 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="5417e-230">Open the MultiInstanceTasks **solution** in Visual Studio 2015 or newer.</span></span> <span data-ttu-id="5417e-231">`MultiInstanceTasks.sln` Řešení soubor je umístěný ve:</span><span class="sxs-lookup"><span data-stu-id="5417e-231">The `MultiInstanceTasks.sln` solution file is located in:</span></span>

    `azure-batch-samples\CSharp\ArticleProjects\MultiInstanceTasks\`
3. <span data-ttu-id="5417e-232">Zadejte přihlašovací údaje účtu Batch a úložiště v `AccountSettings.settings` v **Microsoft.Azure.Batch.Samples.Common** projektu.</span><span class="sxs-lookup"><span data-stu-id="5417e-232">Enter your Batch and Storage account credentials in `AccountSettings.settings` in the **Microsoft.Azure.Batch.Samples.Common** project.</span></span>
4. <span data-ttu-id="5417e-233">**Sestavení a spuštění** řešení MultiInstanceTasks provést MPI ukázkovou aplikaci na výpočetní uzlech ve fondu služby Batch.</span><span class="sxs-lookup"><span data-stu-id="5417e-233">**Build and run** the MultiInstanceTasks solution to execute the MPI sample application on compute nodes in a Batch pool.</span></span>
5. <span data-ttu-id="5417e-234">*Volitelné*: použití [portál Azure] [ portal] nebo [Batch Explorer] [ batch_explorer] prozkoumat ukázkové fondu, úlohy a úkolů (" MultiInstanceSamplePool","MultiInstanceSampleJob","MultiInstanceSampleTask") před odstraněním prostředky.</span><span class="sxs-lookup"><span data-stu-id="5417e-234">*Optional*: Use the [Azure portal][portal] or the [Batch Explorer][batch_explorer] to examine the sample pool, job, and task ("MultiInstanceSamplePool", "MultiInstanceSampleJob", "MultiInstanceSampleTask") before you delete the resources.</span></span>

> [!TIP]
> <span data-ttu-id="5417e-235">Můžete si stáhnout [Visual Studio Community] [ visual_studio] zdarma, pokud nemáte Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5417e-235">You can download [Visual Studio Community][visual_studio] for free if you do not have Visual Studio.</span></span>
>
>

<span data-ttu-id="5417e-236">Výstup z `MultiInstanceTasks.exe` je podobný následujícímu:</span><span class="sxs-lookup"><span data-stu-id="5417e-236">Output from `MultiInstanceTasks.exe` is similar to the following:</span></span>

```
Creating pool [MultiInstanceSamplePool]...
Creating job [MultiInstanceSampleJob]...
Adding task [MultiInstanceSampleTask] to job [MultiInstanceSampleJob]...
Awaiting task completion, timeout in 00:30:00...

Main task [MultiInstanceSampleTask] is in state [Completed] and ran on compute node [tvm-1219235766_1-20161017t162002z]:
---- stdout.txt ----
Rank 2 received string "Hello world" from Rank 0
Rank 1 received string "Hello world" from Rank 0

---- stderr.txt ----

Main task completed, waiting 00:00:10 for subtasks to complete...

---- Subtask information ----
subtask: 1
        exit code: 0
        node: tvm-1219235766_3-20161017t162002z
        stdout.txt:
        stderr.txt:
subtask: 2
        exit code: 0
        node: tvm-1219235766_2-20161017t162002z
        stdout.txt:
        stderr.txt:

Delete job? [yes] no: yes
Delete pool? [yes] no: yes

Sample complete, hit ENTER to exit...
```

## <a name="next-steps"></a><span data-ttu-id="5417e-237">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5417e-237">Next steps</span></span>
* <span data-ttu-id="5417e-238">Popisuje Microsoft HPC & Azure Batch Team blog [MPI podporu pro systém Linux na Azure Batch][blog_mpi_linux]a obsahuje informace o používání [OpenFOAM] [ openfoam] pomocí služby Batch.</span><span class="sxs-lookup"><span data-stu-id="5417e-238">The Microsoft HPC & Azure Batch Team blog discusses [MPI support for Linux on Azure Batch][blog_mpi_linux], and includes information on using [OpenFOAM][openfoam] with Batch.</span></span> <span data-ttu-id="5417e-239">Ukázky kódu Pythonu pro můžete najít [příklad OpenFOAM na Githubu][github_mpi].</span><span class="sxs-lookup"><span data-stu-id="5417e-239">You can find Python code samples for the [OpenFOAM example on GitHub][github_mpi].</span></span>
* <span data-ttu-id="5417e-240">Zjistěte, jak [vytvořit fondy Linuxových výpočetních uzlů](batch-linux-nodes.md) pro použití v Azure Batch MPI řešení.</span><span class="sxs-lookup"><span data-stu-id="5417e-240">Learn how to [create pools of Linux compute nodes](batch-linux-nodes.md) for use in your Azure Batch MPI solutions.</span></span>

[helloworld_proj]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/MultiInstanceTasks/MPIHelloWorld

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[batch_explorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[blog_mpi_linux]: https://blogs.technet.microsoft.com/windowshpc/2016/07/20/introducing-mpi-support-for-linux-on-azure-batch/
[cmd_start]: https://technet.microsoft.com/library/cc770297.aspx
[coord_cmd_example]: https://github.com/Azure/azure-batch-samples/blob/master/Python/Batch/article_samples/mpi/data/linux/openfoam/coordination-cmd
[github_mpi]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/MultiInstanceTasks
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_samples_zip]: https://github.com/Azure/azure-batch-samples/archive/master.zip
[msdn_env_var]: https://msdn.microsoft.com/library/azure/mt743623.aspx
[msmpi_msdn]: https://msdn.microsoft.com/library/bb524831.aspx
[msmpi_sdk]: http://go.microsoft.com/FWLink/p/?LinkID=389556
[msmpi_howto]: http://blogs.technet.com/b/windowshpc/archive/2015/02/02/how-to-compile-and-run-a-simple-ms-mpi-program.aspx
[openfoam]: http://www.openfoam.com/
[visual_studio]: https://www.visualstudio.com/vs/community/

[net_jobprep]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobpreparationtask.aspx
[net_multiinstance_class]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.aspx
[net_multiinstance_prop]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.multiinstancesettings.aspx
[net_multiinsance_commonresfiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.commonresourcefiles.aspx
[net_multiinstance_coordcmdline]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.coordinationcommandline.aspx
[net_multiinstance_numinstances]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.numberofinstances.aspx
[net_pool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_pool_create]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.createpool.aspx
[net_pool_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.starttask.aspx
[net_resourcefile]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.resourcefile.aspx
[net_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.starttask.aspx
[net_starttask_cmdline]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.starttask.commandline.aspx
[net_task]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx
[net_taskconstraints]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskconstraints.aspx
[net_taskconstraint_maxretry]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskconstraints.maxtaskretrycount.aspx
[net_taskconstraint_maxwallclock]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskconstraints.maxwallclocktime.aspx
[net_taskconstraint_retention]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskconstraints.retentiontime.aspx
[net_task_listsubtasks]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.listsubtasks.aspx
[net_task_listnodefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.listnodefiles.aspx
[poolops_getnodefile]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.getnodefile.aspx

[portal]: https://portal.azure.com
[rest_multiinstance]: https://msdn.microsoft.com/library/azure/mt637905.aspx

[1]: ./media/batch-mpi/batch_mpi_01.png "Přehled s více instancemi"
