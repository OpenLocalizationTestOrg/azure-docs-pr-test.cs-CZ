---
title: "s více instancemi aaaUse úloh aplikací MPI toorun - Azure Batch | Microsoft Docs"
description: "Zjistěte, jak v Azure Batch zadejte tooexecute aplikací rozhraní MPI (Message Passing) pomocí hello úkol s více instancemi."
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
ms.openlocfilehash: b0e3295a6aeb76267c26d5504bcff59de3dc5e22
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-multi-instance-tasks-toorun-message-passing-interface-mpi-applications-in-batch"></a><span data-ttu-id="47672-103">Použít s více instancemi úlohy toorun rozhraní MPI (Message Passing) aplikace ve službě Batch</span><span class="sxs-lookup"><span data-stu-id="47672-103">Use multi-instance tasks toorun Message Passing Interface (MPI) applications in Batch</span></span>

<span data-ttu-id="47672-104">Úkoly s více instancemi umožní toorun úloh Azure Batch na několika výpočetních uzlech současně.</span><span class="sxs-lookup"><span data-stu-id="47672-104">Multi-instance tasks allow you toorun an Azure Batch task on multiple compute nodes simultaneously.</span></span> <span data-ttu-id="47672-105">Tyto úlohy povolit scénáře jako aplikací rozhraní MPI (Message Passing) v dávce s vysokým výkonem.</span><span class="sxs-lookup"><span data-stu-id="47672-105">These tasks enable high performance computing scenarios like Message Passing Interface (MPI) applications in Batch.</span></span> <span data-ttu-id="47672-106">V tomto článku se dozvíte, jak pomocí úkolů s více instancemi tooexecute hello [Batch .NET] [ api_net] knihovny.</span><span class="sxs-lookup"><span data-stu-id="47672-106">In this article, you learn how tooexecute multi-instance tasks using hello [Batch .NET][api_net] library.</span></span>

> [!NOTE]
> <span data-ttu-id="47672-107">Zatímco hello příklady v tomto článku se zaměřují na Batch .NET, MS-MPI, a Windows výpočetní uzly, jsou hello s více instancemi úloh Principy probírané v tomto poli použít tooother platformy a technologie (Python a Intel MPI na uzlech Linux, např.).</span><span class="sxs-lookup"><span data-stu-id="47672-107">While hello examples in this article focus on Batch .NET, MS-MPI, and Windows compute nodes, hello multi-instance task concepts discussed here are applicable tooother platforms and technologies (Python and Intel MPI on Linux nodes, for example).</span></span>
>
>

## <a name="multi-instance-task-overview"></a><span data-ttu-id="47672-108">Přehled úloh s více instancemi</span><span class="sxs-lookup"><span data-stu-id="47672-108">Multi-instance task overview</span></span>
<span data-ttu-id="47672-109">Ve službě Batch, je obvykle každý úkol spustit na jednom výpočetním uzlu – odeslat úlohu tooa více úloh, a hello služba Batch plánuje každý úkol pro spuštění na uzlu.</span><span class="sxs-lookup"><span data-stu-id="47672-109">In Batch, each task is normally executed on a single compute node--you submit multiple tasks tooa job, and hello Batch service schedules each task for execution on a node.</span></span> <span data-ttu-id="47672-110">Ale nakonfigurováním úkolu **s více instancemi nastavení**, řekněte Batch tooinstead vytvořit jednu primární úlohu a několik dílčích úkolů, které jsou poté provedeny ve více uzlech.</span><span class="sxs-lookup"><span data-stu-id="47672-110">However, by configuring a task's **multi-instance settings**, you tell Batch tooinstead create one primary task and several subtasks that are then executed on multiple nodes.</span></span>

<span data-ttu-id="47672-111">![Přehled úloh s více instancemi][1]</span><span class="sxs-lookup"><span data-stu-id="47672-111">![Multi-instance task overview][1]</span></span>

<span data-ttu-id="47672-112">Při odesílání úlohy s více instancemi nastavení tooa úlohy Batch provádí úlohy jedinečný toomulti instance několik kroků:</span><span class="sxs-lookup"><span data-stu-id="47672-112">When you submit a task with multi-instance settings tooa job, Batch performs several steps unique toomulti-instance tasks:</span></span>

1. <span data-ttu-id="47672-113">Hello služba Batch vytvoří jednu **primární** a několik **dílčí úkoly** na základě nastavení hello s více instancemi.</span><span class="sxs-lookup"><span data-stu-id="47672-113">hello Batch service creates one **primary** and several **subtasks** based on hello multi-instance settings.</span></span> <span data-ttu-id="47672-114">Celkový počet úloh (primární plus všechny dílčí úkoly) Hello odpovídá hello počet **instance** (výpočetní uzly) určíte v nastavení hello s více instancemi.</span><span class="sxs-lookup"><span data-stu-id="47672-114">hello total number of tasks (primary plus all subtasks) matches hello number of **instances** (compute nodes) you specify in hello multi-instance settings.</span></span>
2. <span data-ttu-id="47672-115">Batch označí jeden hello výpočetní uzly jako hello **hlavní**, a plány hello tooexecute primární úlohy na hlavní server hello.</span><span class="sxs-lookup"><span data-stu-id="47672-115">Batch designates one of hello compute nodes as hello **master**, and schedules hello primary task tooexecute on hello master.</span></span> <span data-ttu-id="47672-116">Naplánuje hello tooexecute dílčí úkoly na hello zbytek hello výpočetní uzly přidělené toohello úkol s více instancemi, jeden dílčí úlohy na uzlu.</span><span class="sxs-lookup"><span data-stu-id="47672-116">It schedules hello subtasks tooexecute on hello remainder of hello compute nodes allocated toohello multi-instance task, one subtask per node.</span></span>
3. <span data-ttu-id="47672-117">Hello primární a všechny dílčí úkoly stahovat **společné soubory prostředků** určíte v nastavení hello s více instancemi.</span><span class="sxs-lookup"><span data-stu-id="47672-117">hello primary and all subtasks download any **common resource files** you specify in hello multi-instance settings.</span></span>
4. <span data-ttu-id="47672-118">Po stažení společné soubory prostředků hello hello primární a dílčí úkoly spusťte hello **koordinaci příkaz** určíte v nastavení hello s více instancemi.</span><span class="sxs-lookup"><span data-stu-id="47672-118">After hello common resource files have been downloaded, hello primary and subtasks execute hello **coordination command** you specify in hello multi-instance settings.</span></span> <span data-ttu-id="47672-119">příkaz koordinaci Hello je obvykle používanými tooprepare uzlů pro provádění úlohy hello.</span><span class="sxs-lookup"><span data-stu-id="47672-119">hello coordination command is typically used tooprepare nodes for executing hello task.</span></span> <span data-ttu-id="47672-120">To může zahrnovat spouštění služby na pozadí (například [Microsoft MPI][msmpi_msdn]na `smpd.exe`) a ověření, že jsou hello uzly připravené tooprocess mezi uzly zprávy.</span><span class="sxs-lookup"><span data-stu-id="47672-120">This can include starting background services (such as [Microsoft MPI][msmpi_msdn]'s `smpd.exe`) and verifying that hello nodes are ready tooprocess inter-node messages.</span></span>
5. <span data-ttu-id="47672-121">hello primární úkol Hello **příkaz aplikace** na hlavní uzel hello *po* příkaz koordinaci hello byla úspěšně dokončena, hello primární a všechny dílčí úkoly.</span><span class="sxs-lookup"><span data-stu-id="47672-121">hello primary task executes hello **application command** on hello master node *after* hello coordination command has been completed successfully by hello primary and all subtasks.</span></span> <span data-ttu-id="47672-122">příkaz aplikace Hello je úkol s více instancemi hello samotné hello příkazový řádek a spouští pouze primární úloh hello.</span><span class="sxs-lookup"><span data-stu-id="47672-122">hello application command is hello command line of hello multi-instance task itself, and is executed only by hello primary task.</span></span> <span data-ttu-id="47672-123">V [MS-MPI][msmpi_msdn]-řešení, to je, kde můžete spuštění vaší aplikace s povolenými MPI pomocí `mpiexec.exe`.</span><span class="sxs-lookup"><span data-stu-id="47672-123">In an [MS-MPI][msmpi_msdn]-based solution, this is where you execute your MPI-enabled application using `mpiexec.exe`.</span></span>

> [!NOTE]
> <span data-ttu-id="47672-124">Když je funkčně distinct, hello "úkol s více instancemi" není typu jedinečný úloh jako hello [StartTask] [ net_starttask] nebo [JobPreparationTask] [ net_jobprep].</span><span class="sxs-lookup"><span data-stu-id="47672-124">Though it is functionally distinct, hello "multi-instance task" is not a unique task type like hello [StartTask][net_starttask] or [JobPreparationTask][net_jobprep].</span></span> <span data-ttu-id="47672-125">úkol s více instancemi Hello je jednoduše standardní úlohy Batch ([CloudTask] [ net_task] v Batch .NET) byla nakonfigurována nastavení s více instancemi.</span><span class="sxs-lookup"><span data-stu-id="47672-125">hello multi-instance task is simply a standard Batch task ([CloudTask][net_task] in Batch .NET) whose multi-instance settings have been configured.</span></span> <span data-ttu-id="47672-126">V tomto článku, označujeme jako hello toothis **úkol s více instancemi**.</span><span class="sxs-lookup"><span data-stu-id="47672-126">In this article, we refer toothis as hello **multi-instance task**.</span></span>
>
>

## <a name="requirements-for-multi-instance-tasks"></a><span data-ttu-id="47672-127">Požadavky pro úkoly s více instancemi</span><span class="sxs-lookup"><span data-stu-id="47672-127">Requirements for multi-instance tasks</span></span>
<span data-ttu-id="47672-128">Úkoly s více instancemi vyžadují fond s **komunikaci mezi uzly povolena**a s **provedení souběžné úlohy zakázáno**.</span><span class="sxs-lookup"><span data-stu-id="47672-128">Multi-instance tasks require a pool with **inter-node communication enabled**, and with **concurrent task execution disabled**.</span></span> <span data-ttu-id="47672-129">provedení souběžné úlohy toodisable, sada hello [CloudPool.MaxTasksPerComputeNode](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool#Microsoft_Azure_Batch_CloudPool_MaxTasksPerComputeNode) too1 vlastnost.</span><span class="sxs-lookup"><span data-stu-id="47672-129">toodisable concurrent task execution, set hello [CloudPool.MaxTasksPerComputeNode](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool#Microsoft_Azure_Batch_CloudPool_MaxTasksPerComputeNode) property too1.</span></span>

<span data-ttu-id="47672-130">Tento fragment kódu ukazuje, jak toocreate fond pro více instancemi úloh, pomocí knihovny Batch .NET hello.</span><span class="sxs-lookup"><span data-stu-id="47672-130">This code snippet shows how toocreate a pool for multi-instance tasks using hello Batch .NET library.</span></span>

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
> <span data-ttu-id="47672-131">Pokud se pokusíte toorun s více instancemi úloha ve fondu s komunikací internodium zakázána, nebo s *maxTasksPerNode* hodnotu větší než 1, hello úloh není vůbec naplánováno – zůstane po neomezenou dobu ve stavu "aktivní" hello.</span><span class="sxs-lookup"><span data-stu-id="47672-131">If you try toorun a multi-instance task in a pool with internode communication disabled, or with a *maxTasksPerNode* value greater than 1, hello task is never scheduled--it remains indefinitely in hello "active" state.</span></span> 
>
> <span data-ttu-id="47672-132">Úkoly s více instancemi lze spustit pouze v uzlů ve fondech vytvořený po 14. prosince 2015.</span><span class="sxs-lookup"><span data-stu-id="47672-132">Multi-instance tasks can execute only on nodes in pools created after 14 December 2015.</span></span>
>
>

### <a name="use-a-starttask-tooinstall-mpi"></a><span data-ttu-id="47672-133">Použít StartTask tooinstall MPI</span><span class="sxs-lookup"><span data-stu-id="47672-133">Use a StartTask tooinstall MPI</span></span>
<span data-ttu-id="47672-134">toorun aplikací MPI s více instancemi úloh, musíte nejdřív tooinstall implementace MPI (MS-MPI nebo MPI Intel, např.) na hello výpočetních uzlů ve fondu hello.</span><span class="sxs-lookup"><span data-stu-id="47672-134">toorun MPI applications with a multi-instance task, you first need tooinstall an MPI implementation (MS-MPI or Intel MPI, for example) on hello compute nodes in hello pool.</span></span> <span data-ttu-id="47672-135">Toto je vhodná doba toouse [StartTask][net_starttask], který provede vždy, když se uzel připojí fondu nebo je restartovat.</span><span class="sxs-lookup"><span data-stu-id="47672-135">This is a good time toouse a [StartTask][net_starttask], which executes whenever a node joins a pool, or is restarted.</span></span> <span data-ttu-id="47672-136">Tento fragment kódu vytvoří StartTask, která určuje instalační balíček hello MS-MPI jako [souboru prostředků][net_resourcefile].</span><span class="sxs-lookup"><span data-stu-id="47672-136">This code snippet creates a StartTask that specifies hello MS-MPI setup package as a [resource file][net_resourcefile].</span></span> <span data-ttu-id="47672-137">Hello spouštěcí úkol příkazový řádek se spustí po souboru prostředků hello stažené toohello uzlu.</span><span class="sxs-lookup"><span data-stu-id="47672-137">hello start task's command line is executed after hello resource file is downloaded toohello node.</span></span> <span data-ttu-id="47672-138">V takovém případě hello příkazový řádek provede bezobslužnou instalaci MS MPI.</span><span class="sxs-lookup"><span data-stu-id="47672-138">In this case, hello command line performs an unattended install of MS-MPI.</span></span>

```csharp
// Create a StartTask for hello pool which we use for installing MS-MPI on
// hello nodes as they join hello pool (or when they are restarted).
StartTask startTask = new StartTask
{
    CommandLine = "cmd /c MSMpiSetup.exe -unattend -force",
    ResourceFiles = new List<ResourceFile> { new ResourceFile("https://mystorageaccount.blob.core.windows.net/mycontainer/MSMpiSetup.exe", "MSMpiSetup.exe") },
    UserIdentity = new UserIdentity(new AutoUserSpecification(elevationLevel: ElevationLevel.Admin)),
    WaitForSuccess = true
};
myCloudPool.StartTask = startTask;

// Commit hello fully configured pool toohello Batch service tooactually create
// hello pool and its compute nodes.
await myCloudPool.CommitAsync();
```

### <a name="remote-direct-memory-access-rdma"></a><span data-ttu-id="47672-139">Vzdálený přímý přístup do paměti (vzdáleného počítače RDMA)</span><span class="sxs-lookup"><span data-stu-id="47672-139">Remote direct memory access (RDMA)</span></span>
<span data-ttu-id="47672-140">Pokud vyberete [RDMA podporovat velikost](../virtual-machines/windows/sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) , jako je A9 pro hello výpočetní uzly ve fondu Batch, MPI aplikace mohou využít výhod Azure vysoce výkonné, nízkou latencí paměti vzdálený přímý přístup do (počítače RDMA) sítě.</span><span class="sxs-lookup"><span data-stu-id="47672-140">When you choose an [RDMA-capable size](../virtual-machines/windows/sizes-hpc.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) such as A9 for hello compute nodes in your Batch pool, your MPI application can take advantage of Azure's high-performance, low-latency remote direct memory access (RDMA) network.</span></span>

<span data-ttu-id="47672-141">Podívejte se na velikosti hello definované jako "Podporující RDMA" v hello následující články:</span><span class="sxs-lookup"><span data-stu-id="47672-141">Look for hello sizes specified as "RDMA capable" in hello following articles:</span></span>

* <span data-ttu-id="47672-142">**CloudServiceConfiguration** fondy</span><span class="sxs-lookup"><span data-stu-id="47672-142">**CloudServiceConfiguration** pools</span></span>

  * <span data-ttu-id="47672-143">[Velikosti pro cloudové služby](../cloud-services/cloud-services-sizes-specs.md) (jenom Windows)</span><span class="sxs-lookup"><span data-stu-id="47672-143">[Sizes for Cloud Services](../cloud-services/cloud-services-sizes-specs.md) (Windows only)</span></span>
* <span data-ttu-id="47672-144">**VirtualMachineConfiguration** fondy</span><span class="sxs-lookup"><span data-stu-id="47672-144">**VirtualMachineConfiguration** pools</span></span>

  * <span data-ttu-id="47672-145">[Velikosti virtuálních počítačů v Azure](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (Linux)</span><span class="sxs-lookup"><span data-stu-id="47672-145">[Sizes for virtual machines in Azure](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (Linux)</span></span>
  * <span data-ttu-id="47672-146">[Velikosti virtuálních počítačů v Azure](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (Windows)</span><span class="sxs-lookup"><span data-stu-id="47672-146">[Sizes for virtual machines in Azure](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (Windows)</span></span>

> [!NOTE]
> <span data-ttu-id="47672-147">tootake výhod RDMA na [Linuxových výpočetních uzlů](batch-linux-nodes.md), je nutné použít **Intel MPI** na uzlech hello.</span><span class="sxs-lookup"><span data-stu-id="47672-147">tootake advantage of RDMA on [Linux compute nodes](batch-linux-nodes.md), you must use **Intel MPI** on hello nodes.</span></span> <span data-ttu-id="47672-148">Další informace o CloudServiceConfiguration a VirtualMachineConfiguration fondy najdete v tématu hello fondu části hello [přehled funkcí Batch](batch-api-basics.md).</span><span class="sxs-lookup"><span data-stu-id="47672-148">For more information on CloudServiceConfiguration and VirtualMachineConfiguration pools, see hello Pool section of hello [Batch feature overview](batch-api-basics.md).</span></span>
>
>

## <a name="create-a-multi-instance-task-with-batch-net"></a><span data-ttu-id="47672-149">Vytvoření úlohy s více instancemi pomocí rozhraní Batch .NET</span><span class="sxs-lookup"><span data-stu-id="47672-149">Create a multi-instance task with Batch .NET</span></span>
<span data-ttu-id="47672-150">Teď, když jsme si zahrnutých hello fondu požadavky a instalace balíčku MPI, vytvoříme hello úkol s více instancemi.</span><span class="sxs-lookup"><span data-stu-id="47672-150">Now that we've covered hello pool requirements and MPI package installation, let's create hello multi-instance task.</span></span> <span data-ttu-id="47672-151">V tento fragment kódu, vytvoříme standard [CloudTask][net_task], nakonfigurujte její [MultiInstanceSettings] [ net_multiinstance_prop] vlastnost.</span><span class="sxs-lookup"><span data-stu-id="47672-151">In this snippet, we create a standard [CloudTask][net_task], then configure its [MultiInstanceSettings][net_multiinstance_prop] property.</span></span> <span data-ttu-id="47672-152">Jak už bylo zmíněno dříve, není úkol s více instancemi hello typu distinct úloh, ale standardní úlohy Batch nakonfigurované s nastavením s více instancemi.</span><span class="sxs-lookup"><span data-stu-id="47672-152">As mentioned earlier, hello multi-instance task is not a distinct task type, but a standard Batch task configured with multi-instance settings.</span></span>

```csharp
// Create hello multi-instance task. Its command line is hello "application command"
// and will be executed *only* by hello primary, and only after hello primary and
// subtasks execute hello CoordinationCommandLine.
CloudTask myMultiInstanceTask = new CloudTask(id: "mymultiinstancetask",
    commandline: "cmd /c mpiexec.exe -wdir %AZ_BATCH_TASK_SHARED_DIR% MyMPIApplication.exe");

// Configure hello task's MultiInstanceSettings. hello CoordinationCommandLine will be executed by
// hello primary and all subtasks.
myMultiInstanceTask.MultiInstanceSettings =
    new MultiInstanceSettings(numberOfNodes) {
    CoordinationCommandLine = @"cmd /c start cmd /c ""%MSMPI_BIN%\smpd.exe"" -d",
    CommonResourceFiles = new List<ResourceFile> {
    new ResourceFile("https://mystorageaccount.blob.core.windows.net/mycontainer/MyMPIApplication.exe",
                     "MyMPIApplication.exe")
    }
};

// Submit hello task toohello job. Batch will take care of splitting it into subtasks and
// scheduling them for execution on hello nodes.
await myBatchClient.JobOperations.AddTaskAsync("mybatchjob", myMultiInstanceTask);
```

## <a name="primary-task-and-subtasks"></a><span data-ttu-id="47672-153">Primární úlohy a dílčí úkoly</span><span class="sxs-lookup"><span data-stu-id="47672-153">Primary task and subtasks</span></span>
<span data-ttu-id="47672-154">Když vytvoříte hello s více instancemi nastavení pro úlohu, zadáte hello počet výpočetních uzlů, které jsou tooexecute hello úloh.</span><span class="sxs-lookup"><span data-stu-id="47672-154">When you create hello multi-instance settings for a task, you specify hello number of compute nodes that are tooexecute hello task.</span></span> <span data-ttu-id="47672-155">Při odesílání úlohy tooa úloh hello hello služba Batch vytvoří jednu **primární** úlohy a dostatek **dílčí úkoly** společně odpovídající hello počet uzlů, které jste zadali.</span><span class="sxs-lookup"><span data-stu-id="47672-155">When you submit hello task tooa job, hello Batch service creates one **primary** task and enough **subtasks** that together match hello number of nodes you specified.</span></span>

<span data-ttu-id="47672-156">Tyto úlohy jsou přiřazeny id celé číslo v rozsahu 0 hello příliš*numberOfInstances* - 1.</span><span class="sxs-lookup"><span data-stu-id="47672-156">These tasks are assigned an integer id in hello range of 0 too*numberOfInstances* - 1.</span></span> <span data-ttu-id="47672-157">Hello úlohy s id 0 je hello primární úlohy a dílčí úkoly jsou všechny ostatní ID.</span><span class="sxs-lookup"><span data-stu-id="47672-157">hello task with id 0 is hello primary task, and all other ids are subtasks.</span></span> <span data-ttu-id="47672-158">Například pokud vytvoříte hello následující nastavení s více instancemi pro úlohu, hello primární úloh má id 0 a dílčí úkoly hello by měla mít ID 1 až 9.</span><span class="sxs-lookup"><span data-stu-id="47672-158">For example, if you create hello following multi-instance settings for a task, hello primary task would have an id of 0, and hello subtasks would have ids 1 through 9.</span></span>

```csharp
int numberOfNodes = 10;
myMultiInstanceTask.MultiInstanceSettings = new MultiInstanceSettings(numberOfNodes);
```

### <a name="master-node"></a><span data-ttu-id="47672-159">Hlavní uzel</span><span class="sxs-lookup"><span data-stu-id="47672-159">Master node</span></span>
<span data-ttu-id="47672-160">Při odesílání úlohy s více instancemi hello služba Batch označí jeden hello výpočetní uzly jako uzel "hlavní" hello, a plány hello tooexecute primární úlohy na hlavní uzel hello.</span><span class="sxs-lookup"><span data-stu-id="47672-160">When you submit a multi-instance task, hello Batch service designates one of hello compute nodes as hello "master" node, and schedules hello primary task tooexecute on hello master node.</span></span> <span data-ttu-id="47672-161">Hello dílčí úkoly jsou naplánované tooexecute na hello zbytek hello uzlů přidělených toohello úkol s více instancemi.</span><span class="sxs-lookup"><span data-stu-id="47672-161">hello subtasks are scheduled tooexecute on hello remainder of hello nodes allocated toohello multi-instance task.</span></span>

## <a name="coordination-command"></a><span data-ttu-id="47672-162">Příkaz spolupráce</span><span class="sxs-lookup"><span data-stu-id="47672-162">Coordination command</span></span>
<span data-ttu-id="47672-163">Hello **koordinaci příkaz** provedený hello primární i dílčí úkoly.</span><span class="sxs-lookup"><span data-stu-id="47672-163">hello **coordination command** is executed by both hello primary and subtasks.</span></span>

<span data-ttu-id="47672-164">blokuje Hello vyvolání příkazu koordinaci hello – Batch příkaz aplikace hello nespustí, dokud je úspěšně vrácený příkaz hello spolupráce pro všechny dílčí úkoly.</span><span class="sxs-lookup"><span data-stu-id="47672-164">hello invocation of hello coordination command is blocking--Batch does not execute hello application command until hello coordination command has returned successfully for all subtasks.</span></span> <span data-ttu-id="47672-165">příkaz koordinaci Hello by proto spustit žádné služby, požadované pozadí, ověřte, zda je připravený k použití a poté ukončete.</span><span class="sxs-lookup"><span data-stu-id="47672-165">hello coordination command should therefore start any required background services, verify that they are ready for use, and then exit.</span></span> <span data-ttu-id="47672-166">Například tento příkaz koordinaci pro řešení pomocí MS-MPI verze 7 spustí službu SMPD hello na hello uzel, a poté bude ukončen:</span><span class="sxs-lookup"><span data-stu-id="47672-166">For example, this coordination command for a solution using MS-MPI version 7 starts hello SMPD service on hello node, then exits:</span></span>

```
cmd /c start cmd /c ""%MSMPI_BIN%\smpd.exe"" -d
```

<span data-ttu-id="47672-167">Všimněte si použití hello `start` v tomto příkazu spolupráce.</span><span class="sxs-lookup"><span data-stu-id="47672-167">Note hello use of `start` in this coordination command.</span></span> <span data-ttu-id="47672-168">Toto je nutné kvůli hello `smpd.exe` aplikace nevrátí okamžitě po spuštění.</span><span class="sxs-lookup"><span data-stu-id="47672-168">This is required because hello `smpd.exe` application does not return immediately after execution.</span></span> <span data-ttu-id="47672-169">Bez použití hello hello [spustit] [ cmd_start] příkaz, tento příkaz koordinaci by vrátit a by proto blokovat příkaz aplikace hello spuštění.</span><span class="sxs-lookup"><span data-stu-id="47672-169">Without hello use of hello [start][cmd_start] command, this coordination command would not return, and would therefore block hello application command from running.</span></span>

## <a name="application-command"></a><span data-ttu-id="47672-170">Příkaz aplikace</span><span class="sxs-lookup"><span data-stu-id="47672-170">Application command</span></span>
<span data-ttu-id="47672-171">Jakmile hello primární úlohy a všechny dílčí úkoly dokončení provádění příkazu koordinaci hello, úkolů s více instancemi hello příkazový řádek se spustí primární úlohou hello *pouze*.</span><span class="sxs-lookup"><span data-stu-id="47672-171">Once hello primary task and all subtasks have finished executing hello coordination command, hello multi-instance task's command line is executed by hello primary task *only*.</span></span> <span data-ttu-id="47672-172">Říkáme tento hello **příkaz aplikace** toodistinguish z příkazu koordinaci hello.</span><span class="sxs-lookup"><span data-stu-id="47672-172">We call this hello **application command** toodistinguish it from hello coordination command.</span></span>

<span data-ttu-id="47672-173">U aplikací, MS-MPI, použijte hello aplikace příkaz tooexecute vaší aplikace s povolenými MPI s `mpiexec.exe`.</span><span class="sxs-lookup"><span data-stu-id="47672-173">For MS-MPI applications, use hello application command tooexecute your MPI-enabled application with `mpiexec.exe`.</span></span> <span data-ttu-id="47672-174">Zde je ukázka, příkaz aplikace pro řešení pomocí MS-MPI verze 7:</span><span class="sxs-lookup"><span data-stu-id="47672-174">For example, here is an application command for a solution using MS-MPI version 7:</span></span>

```
cmd /c ""%MSMPI_BIN%\mpiexec.exe"" -c 1 -wdir %AZ_BATCH_TASK_SHARED_DIR% MyMPIApplication.exe
```

> [!NOTE]
> <span data-ttu-id="47672-175">Protože MS-MPI `mpiexec.exe` hello používá `CCP_NODES` proměnné ve výchozím nastavení (najdete v části [proměnné prostředí](#environment-variables)) příklad hello aplikace příkazového řádku výše vyloučí ho.</span><span class="sxs-lookup"><span data-stu-id="47672-175">Because MS-MPI's `mpiexec.exe` uses hello `CCP_NODES` variable by default (see [Environment variables](#environment-variables)) hello example application command line above excludes it.</span></span>
>
>

## <a name="environment-variables"></a><span data-ttu-id="47672-176">Proměnné prostředí</span><span class="sxs-lookup"><span data-stu-id="47672-176">Environment variables</span></span>
<span data-ttu-id="47672-177">Batch vytvoří několik [proměnné prostředí] [ msdn_env_var] konkrétní instanci toomulti úlohy na hello výpočetní uzly přiděleny tooa úkol s více instancemi.</span><span class="sxs-lookup"><span data-stu-id="47672-177">Batch creates several [environment variables][msdn_env_var] specific toomulti-instance tasks on hello compute nodes allocated tooa multi-instance task.</span></span> <span data-ttu-id="47672-178">Příkazové řádky koordinace a aplikace můžete odkazovat těchto proměnných prostředí, jak můžete hello skripty a programů, které se provést.</span><span class="sxs-lookup"><span data-stu-id="47672-178">Your coordination and application command lines can reference these environment variables, as can hello scripts and programs they execute.</span></span>

<span data-ttu-id="47672-179">Hello následující proměnné prostředí jsou vytvořené pomocí služby Batch hello pro použití úkolů s více instancemi:</span><span class="sxs-lookup"><span data-stu-id="47672-179">hello following environment variables are created by hello Batch service for use by multi-instance tasks:</span></span>

* `CCP_NODES`
* `AZ_BATCH_NODE_LIST`
* `AZ_BATCH_HOST_LIST`
* `AZ_BATCH_MASTER_NODE`
* `AZ_BATCH_TASK_SHARED_DIR`
* `AZ_BATCH_IS_CURRENT_NODE_MASTER`

<span data-ttu-id="47672-180">Úplné podrobnosti o těchto a hello jiné Batch výpočetní uzel proměnné prostředí, včetně jejich obsah a viditelnost, najdete v části [výpočetní uzel proměnné prostředí][msdn_env_var].</span><span class="sxs-lookup"><span data-stu-id="47672-180">For full details on these and hello other Batch compute node environment variables, including their contents and visibility, see [Compute node environment variables][msdn_env_var].</span></span>

> [!TIP]
> <span data-ttu-id="47672-181">Ukázka kódu Hello Batch Linux MPI obsahuje příklad některé z těchto proměnných prostředí použití.</span><span class="sxs-lookup"><span data-stu-id="47672-181">hello Batch Linux MPI code sample contains an example of how several of these environment variables can be used.</span></span> <span data-ttu-id="47672-182">Hello [koordinaci cmd] [ coord_cmd_example] Bash skript stáhne běžné aplikace a vstupní soubory ze služby Azure Storage, sdílené složky systému souborů NFS v hello hlavní uzel povolí a nakonfiguruje hello jiné uzly úkol s více instancemi toohello přidělené jako klienti NFS.</span><span class="sxs-lookup"><span data-stu-id="47672-182">hello [coordination-cmd][coord_cmd_example] Bash script downloads common application and input files from Azure Storage, enables a Network File System (NFS) share on hello master node, and configures hello other nodes allocated toohello multi-instance task as NFS clients.</span></span>
>
>

## <a name="resource-files"></a><span data-ttu-id="47672-183">Soubory prostředků</span><span class="sxs-lookup"><span data-stu-id="47672-183">Resource files</span></span>
<span data-ttu-id="47672-184">Existují dvě sady tooconsider soubory prostředků pro úkoly s více instancemi: **společné soubory prostředků** , *všechny* úkoly stahují (obě primární a dílčí úkoly) a hello **soubory prostředků** zadaná pro hello s více instancemi úkolů sebe, což *pouze hello primární* úkolů stahování.</span><span class="sxs-lookup"><span data-stu-id="47672-184">There are two sets of resource files tooconsider for multi-instance tasks: **common resource files** that *all* tasks download (both primary and subtasks), and hello **resource files** specified for hello multi-instance task itself, which *only hello primary* task downloads.</span></span>

<span data-ttu-id="47672-185">Můžete určit jeden nebo více **společné soubory prostředků** hello s více instancemi nastavení pro úlohu.</span><span class="sxs-lookup"><span data-stu-id="47672-185">You can specify one or more **common resource files** in hello multi-instance settings for a task.</span></span> <span data-ttu-id="47672-186">Tyto společné soubory prostředků se stahují z [Azure Storage](../storage/common/storage-introduction.md) do každého uzlu **sdíleného adresáře úkolu** hello primární a všechny dílčí úkoly.</span><span class="sxs-lookup"><span data-stu-id="47672-186">These common resource files are downloaded from [Azure Storage](../storage/common/storage-introduction.md) into each node's **task shared directory** by hello primary and all subtasks.</span></span> <span data-ttu-id="47672-187">Dostanete hello úloh sdíleného adresáře z aplikace a koordinaci příkazové řádky pomocí hello `AZ_BATCH_TASK_SHARED_DIR` proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="47672-187">You can access hello task shared directory from application and coordination command lines by using hello `AZ_BATCH_TASK_SHARED_DIR` environment variable.</span></span> <span data-ttu-id="47672-188">Hello `AZ_BATCH_TASK_SHARED_DIR` cesta je stejná na každý úkol s více instancemi přidělené toohello uzlu, takže můžete sdílet příkaz jeden spolupráce mezi hello primární a všechny dílčí úkoly.</span><span class="sxs-lookup"><span data-stu-id="47672-188">hello `AZ_BATCH_TASK_SHARED_DIR` path is identical on every node allocated toohello multi-instance task, thus you can share a single coordination command between hello primary and all subtasks.</span></span> <span data-ttu-id="47672-189">Batch "nesdílí" hello adresáře v tom smyslu vzdáleného přístupu, ale můžete ho použít jako přípojný nebo sdílení bodu, jak je uvedeno výše v hello tip na proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="47672-189">Batch does not "share" hello directory in a remote access sense, but you can use it as a mount or share point as mentioned earlier in hello tip on environment variables.</span></span>

<span data-ttu-id="47672-190">Soubory prostředků, které zadáte pro hello úkol s více instancemi samotné jsou pracovní adresář pro stažené toohello úkolů, `AZ_BATCH_TASK_WORKING_DIR`, ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="47672-190">Resource files that you specify for hello multi-instance task itself are downloaded toohello task's working directory, `AZ_BATCH_TASK_WORKING_DIR`, by default.</span></span> <span data-ttu-id="47672-191">Jak je uvedeno, na rozdíl od toocommon soubory prostředků, pouze hello primární úkol stáhne soubory prostředků, zadaný pro vlastní úloha hello s více instancemi.</span><span class="sxs-lookup"><span data-stu-id="47672-191">As mentioned, in contrast toocommon resource files, only hello primary task downloads resource files specified for hello  multi-instance task itself.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="47672-192">Vždy použít proměnné prostředí hello `AZ_BATCH_TASK_SHARED_DIR` a `AZ_BATCH_TASK_WORKING_DIR` toorefer toothese adresářů v příkazové řádky.</span><span class="sxs-lookup"><span data-stu-id="47672-192">Always use hello environment variables `AZ_BATCH_TASK_SHARED_DIR` and `AZ_BATCH_TASK_WORKING_DIR` toorefer toothese directories in your command lines.</span></span> <span data-ttu-id="47672-193">Ručně nepokoušejte tooconstruct hello cesty.</span><span class="sxs-lookup"><span data-stu-id="47672-193">Do not attempt tooconstruct hello paths manually.</span></span>
>
>

## <a name="task-lifetime"></a><span data-ttu-id="47672-194">Doba platnosti úloh</span><span class="sxs-lookup"><span data-stu-id="47672-194">Task lifetime</span></span>
<span data-ttu-id="47672-195">Doba platnosti Hello životnosti hello primární úloh Ovládací prvky hello hello celý s více instancemi úlohy.</span><span class="sxs-lookup"><span data-stu-id="47672-195">hello lifetime of hello primary task controls hello lifetime of hello entire multi-instance task.</span></span> <span data-ttu-id="47672-196">Při ukončení hello primární, všechny dílčí úkoly hello ukončena.</span><span class="sxs-lookup"><span data-stu-id="47672-196">When hello primary exits, all of hello subtasks are terminated.</span></span> <span data-ttu-id="47672-197">ukončovací kód Hello hello primární je hello ukončovací kód hello úkolů a je proto použité toodetermine hello úspěch nebo neúspěch hello úlohy pro účely opakování.</span><span class="sxs-lookup"><span data-stu-id="47672-197">hello exit code of hello primary is hello exit code of hello task, and is therefore used toodetermine hello success or failure of hello task for retry purposes.</span></span>

<span data-ttu-id="47672-198">Pokud žádný z dílčích úkolů hello selže, ukončení s návratovým kódem nulová, například hello celý s více instancemi úloha nezdaří.</span><span class="sxs-lookup"><span data-stu-id="47672-198">If any of hello subtasks fail, exiting with a non-zero return code, for example, hello entire multi-instance task fails.</span></span> <span data-ttu-id="47672-199">úkol s více instancemi Hello je pak byla ukončena a opakovat, až tooits limit opakování.</span><span class="sxs-lookup"><span data-stu-id="47672-199">hello multi-instance task is then terminated and retried, up tooits retry limit.</span></span>

<span data-ttu-id="47672-200">Při odstranění úlohu s více instancemi podle hello služba Batch odstranit také hello primární a všechny dílčí úkoly.</span><span class="sxs-lookup"><span data-stu-id="47672-200">When you delete a multi-instance task, hello primary and all subtasks are also deleted by hello Batch service.</span></span> <span data-ttu-id="47672-201">Všechny dílčí úloha adresáře a jejich soubory se odstraní z hello výpočetních uzlů, stejně jako u standardní úlohy.</span><span class="sxs-lookup"><span data-stu-id="47672-201">All subtask directories and their files are deleted from hello compute nodes, just as for a standard task.</span></span>

<span data-ttu-id="47672-202">[TaskConstraints] [ net_taskconstraints] pro úlohu s více instancemi, třeba hello [MaxTaskRetryCount][net_taskconstraint_maxretry], [MaxWallClockTime] [ net_taskconstraint_maxwallclock], a [RetentionTime] [ net_taskconstraint_retention] vlastnosti, se neuplatňují, jak jsou pro standardní úlohy a použít toohello primární server a všechny dílčí úkoly.</span><span class="sxs-lookup"><span data-stu-id="47672-202">[TaskConstraints][net_taskconstraints] for a multi-instance task, such as hello [MaxTaskRetryCount][net_taskconstraint_maxretry], [MaxWallClockTime][net_taskconstraint_maxwallclock], and [RetentionTime][net_taskconstraint_retention] properties, are honored as they are for a standard task, and apply toohello primary and all subtasks.</span></span> <span data-ttu-id="47672-203">Nicméně pokud změníte hello [RetentionTime] [ net_taskconstraint_retention] vlastnost po přidání hello s více instancemi úloh toohello úlohy, tato změna je použité pouze toohello primární úloh.</span><span class="sxs-lookup"><span data-stu-id="47672-203">However, if you change hello [RetentionTime][net_taskconstraint_retention] property after adding hello multi-instance task toohello job, this change is applied only toohello primary task.</span></span> <span data-ttu-id="47672-204">Všechny dílčí úkoly hello pokračovat původní hello toouse [RetentionTime][net_taskconstraint_retention].</span><span class="sxs-lookup"><span data-stu-id="47672-204">All of hello subtasks continue toouse hello original [RetentionTime][net_taskconstraint_retention].</span></span>

<span data-ttu-id="47672-205">Seznam posledních úkolů výpočetním uzlu odráží hello id dílčí úlohy, pokud poslední úloha hello byla součástí úlohu s více instancemi.</span><span class="sxs-lookup"><span data-stu-id="47672-205">A compute node's recent task list reflects hello id of a subtask if hello recent task was part of a multi-instance task.</span></span>

## <a name="obtain-information-about-subtasks"></a><span data-ttu-id="47672-206">Získání informací o dílčí úkoly</span><span class="sxs-lookup"><span data-stu-id="47672-206">Obtain information about subtasks</span></span>
<span data-ttu-id="47672-207">informace o tooobtain na dílčí úkoly pomocí knihovny Batch .NET hello, volání hello [CloudTask.ListSubtasks] [ net_task_listsubtasks] metoda.</span><span class="sxs-lookup"><span data-stu-id="47672-207">tooobtain information on subtasks by using hello Batch .NET library, call hello [CloudTask.ListSubtasks][net_task_listsubtasks] method.</span></span> <span data-ttu-id="47672-208">Tato metoda vrátí informace na všechny dílčí úkoly a informace o hello výpočetního uzlu, který spuštěné úlohy hello.</span><span class="sxs-lookup"><span data-stu-id="47672-208">This method returns information on all subtasks, and information about hello compute node that executed hello tasks.</span></span> <span data-ttu-id="47672-209">Z těchto informací můžete určit každý dílčí úlohy kořenový adresář, hello id fondu, jeho aktuální stav, ukončovacího kódu a další.</span><span class="sxs-lookup"><span data-stu-id="47672-209">From this information, you can determine each subtask's root directory, hello pool id, its current state, exit code, and more.</span></span> <span data-ttu-id="47672-210">Tyto informace můžete použít v kombinaci s hello [PoolOperations.GetNodeFile] [ poolops_getnodefile] metoda tooobtain hello dílčí úlohy na soubory.</span><span class="sxs-lookup"><span data-stu-id="47672-210">You can use this information in combination with hello [PoolOperations.GetNodeFile][poolops_getnodefile] method tooobtain hello subtask's files.</span></span> <span data-ttu-id="47672-211">Všimněte si, že tato metoda nevrátí informace pro primární úlohu hello (id 0).</span><span class="sxs-lookup"><span data-stu-id="47672-211">Note that this method does not return information for hello primary task (id 0).</span></span>

> [!NOTE]
> <span data-ttu-id="47672-212">Pokud není uvedeno jinak, metody rozhraní Batch .NET, které působí na hello s více instancemi [CloudTask] [ net_task] samotné použít *pouze* toohello primární úloh.</span><span class="sxs-lookup"><span data-stu-id="47672-212">Unless otherwise stated, Batch .NET methods that operate on hello multi-instance [CloudTask][net_task] itself apply *only* toohello primary task.</span></span> <span data-ttu-id="47672-213">Například při volání hello [CloudTask.ListNodeFiles] [ net_task_listnodefiles] metodu úlohu s více instancemi, jsou vráceny pouze soubory hello primární úkolu.</span><span class="sxs-lookup"><span data-stu-id="47672-213">For example, when you call hello [CloudTask.ListNodeFiles][net_task_listnodefiles] method on a multi-instance task, only hello primary task's files are returned.</span></span>
>
>

<span data-ttu-id="47672-214">Hello následující fragment kódu ukazuje, jak tooobtain dílčí úloha informace, a také žádosti o obsah souboru z hello uzlů, ve kterých provést.</span><span class="sxs-lookup"><span data-stu-id="47672-214">hello following code snippet shows how tooobtain subtask information, as well as request file contents from hello nodes on which they executed.</span></span>

```csharp
// Obtain hello job and hello multi-instance task from hello Batch service
CloudJob boundJob = batchClient.JobOperations.GetJob("mybatchjob");
CloudTask myMultiInstanceTask = boundJob.GetTask("mymultiinstancetask");

// Now obtain hello list of subtasks for hello task
IPagedEnumerable<SubtaskInformation> subtasks = myMultiInstanceTask.ListSubtasks();

// Asynchronously iterate over hello subtasks and print their stdout and stderr
// output if hello subtask has completed
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

## <a name="code-sample"></a><span data-ttu-id="47672-215">Ukázka kódu</span><span class="sxs-lookup"><span data-stu-id="47672-215">Code sample</span></span>
<span data-ttu-id="47672-216">Hello [MultiInstanceTasks] [ github_mpi] ukázka kódu na Githubu ukazuje, jak toouse s více instancemi úkolů toorun [MS-MPI] [ msmpi_msdn] aplikace na výpočetních uzlech Batch.</span><span class="sxs-lookup"><span data-stu-id="47672-216">hello [MultiInstanceTasks][github_mpi] code sample on GitHub demonstrates how toouse a multi-instance task toorun an [MS-MPI][msmpi_msdn] application on Batch compute nodes.</span></span> <span data-ttu-id="47672-217">Postupujte podle kroků hello v [přípravy](#preparation) a [provádění](#execution) toorun hello ukázka.</span><span class="sxs-lookup"><span data-stu-id="47672-217">Follow hello steps in [Preparation](#preparation) and [Execution](#execution) toorun hello sample.</span></span>

### <a name="preparation"></a><span data-ttu-id="47672-218">Příprava</span><span class="sxs-lookup"><span data-stu-id="47672-218">Preparation</span></span>
1. <span data-ttu-id="47672-219">Postupujte podle hello první dva kroky v [jak toocompile a spusťte jednoduchý program MS-MPI][msmpi_howto].</span><span class="sxs-lookup"><span data-stu-id="47672-219">Follow hello first two steps in [How toocompile and run a simple MS-MPI program][msmpi_howto].</span></span> <span data-ttu-id="47672-220">Splňuje to, že hello prerequesites pro hello následující krok.</span><span class="sxs-lookup"><span data-stu-id="47672-220">This satisfies hello prerequesites for hello following step.</span></span>
2. <span data-ttu-id="47672-221">Sestavení *verze* verzi hello [MPIHelloWorld] [ helloworld_proj] ukázka MPI programu.</span><span class="sxs-lookup"><span data-stu-id="47672-221">Build a *Release* version of hello [MPIHelloWorld][helloworld_proj] sample MPI program.</span></span> <span data-ttu-id="47672-222">Toto je hello program, který se má spustit na výpočetních uzlech úkol s více instancemi hello.</span><span class="sxs-lookup"><span data-stu-id="47672-222">This is hello program that will be run on compute nodes by hello multi-instance task.</span></span>
3. <span data-ttu-id="47672-223">Vytvořte soubor zip obsahující `MPIHelloWorld.exe` (které je vytvořené v kroku 2) a `MSMpiSetup.exe` (který jste stáhli krok 1).</span><span class="sxs-lookup"><span data-stu-id="47672-223">Create a zip file containing `MPIHelloWorld.exe` (which you built step 2) and `MSMpiSetup.exe` (which you downloaded step 1).</span></span> <span data-ttu-id="47672-224">Tento soubor zip budete nahrát jako balíček aplikace v dalším kroku hello.</span><span class="sxs-lookup"><span data-stu-id="47672-224">You'll upload this zip file as an application package in hello next step.</span></span>
4. <span data-ttu-id="47672-225">Použití hello [portál Azure] [ portal] toocreate dávky [aplikace](batch-application-packages.md) nazývá "MPIHelloWorld" a zadejte soubor zip hello jste vytvořili v předchozím kroku hello jako verze "1.0" balíček aplikace Hello.</span><span class="sxs-lookup"><span data-stu-id="47672-225">Use hello [Azure portal][portal] toocreate a Batch [application](batch-application-packages.md) called "MPIHelloWorld", and specify hello zip file you created in hello previous step as version "1.0" of hello application package.</span></span> <span data-ttu-id="47672-226">V tématu [odesílat a spravovat aplikace](batch-application-packages.md#upload-and-manage-applications) Další informace.</span><span class="sxs-lookup"><span data-stu-id="47672-226">See [Upload and manage applications](batch-application-packages.md#upload-and-manage-applications) for more information.</span></span>

> [!TIP]
> <span data-ttu-id="47672-227">Sestavení *verze* verzi `MPIHelloWorld.exe` tak, že nemáte tooinclude žádné další závislosti (například `msvcp140d.dll` nebo `vcruntime140d.dll`) v balíčku aplikace.</span><span class="sxs-lookup"><span data-stu-id="47672-227">Build a *Release* version of `MPIHelloWorld.exe` so that you don't have tooinclude any additional dependencies (for example, `msvcp140d.dll` or `vcruntime140d.dll`) in your application package.</span></span>
>
>

### <a name="execution"></a><span data-ttu-id="47672-228">Provádění</span><span class="sxs-lookup"><span data-stu-id="47672-228">Execution</span></span>
1. <span data-ttu-id="47672-229">Stáhnout hello [azure-batch-samples] [ github_samples_zip] z Githubu.</span><span class="sxs-lookup"><span data-stu-id="47672-229">Download hello [azure-batch-samples][github_samples_zip] from GitHub.</span></span>
2. <span data-ttu-id="47672-230">Otevřete hello MultiInstanceTasks **řešení** v sadě Visual Studio 2015 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="47672-230">Open hello MultiInstanceTasks **solution** in Visual Studio 2015 or newer.</span></span> <span data-ttu-id="47672-231">Hello `MultiInstanceTasks.sln` řešení soubor je umístěný ve:</span><span class="sxs-lookup"><span data-stu-id="47672-231">hello `MultiInstanceTasks.sln` solution file is located in:</span></span>

    `azure-batch-samples\CSharp\ArticleProjects\MultiInstanceTasks\`
3. <span data-ttu-id="47672-232">Zadejte přihlašovací údaje účtu Batch a úložiště v `AccountSettings.settings` v hello **Microsoft.Azure.Batch.Samples.Common** projektu.</span><span class="sxs-lookup"><span data-stu-id="47672-232">Enter your Batch and Storage account credentials in `AccountSettings.settings` in hello **Microsoft.Azure.Batch.Samples.Common** project.</span></span>
4. <span data-ttu-id="47672-233">**Sestavení a spuštění** hello MultiInstanceTasks řešení tooexecute hello MPI ukázkovou aplikaci na výpočetních uzlů ve fondu služby Batch.</span><span class="sxs-lookup"><span data-stu-id="47672-233">**Build and run** hello MultiInstanceTasks solution tooexecute hello MPI sample application on compute nodes in a Batch pool.</span></span>
5. <span data-ttu-id="47672-234">*Volitelné*: použití hello [portál Azure] [ portal] nebo hello [Batch Explorer] [ batch_explorer] tooexamine hello ukázka fondu, úlohy, a úloha ("MultiInstanceSamplePool", "MultiInstanceSampleJob", "MultiInstanceSampleTask") před odstranit hello prostředky.</span><span class="sxs-lookup"><span data-stu-id="47672-234">*Optional*: Use hello [Azure portal][portal] or hello [Batch Explorer][batch_explorer] tooexamine hello sample pool, job, and task ("MultiInstanceSamplePool", "MultiInstanceSampleJob", "MultiInstanceSampleTask") before you delete hello resources.</span></span>

> [!TIP]
> <span data-ttu-id="47672-235">Můžete si stáhnout [Visual Studio Community] [ visual_studio] zdarma, pokud nemáte Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="47672-235">You can download [Visual Studio Community][visual_studio] for free if you do not have Visual Studio.</span></span>
>
>

<span data-ttu-id="47672-236">Výstup z `MultiInstanceTasks.exe` je podobné toohello následující:</span><span class="sxs-lookup"><span data-stu-id="47672-236">Output from `MultiInstanceTasks.exe` is similar toohello following:</span></span>

```
Creating pool [MultiInstanceSamplePool]...
Creating job [MultiInstanceSampleJob]...
Adding task [MultiInstanceSampleTask] toojob [MultiInstanceSampleJob]...
Awaiting task completion, timeout in 00:30:00...

Main task [MultiInstanceSampleTask] is in state [Completed] and ran on compute node [tvm-1219235766_1-20161017t162002z]:
---- stdout.txt ----
Rank 2 received string "Hello world" from Rank 0
Rank 1 received string "Hello world" from Rank 0

---- stderr.txt ----

Main task completed, waiting 00:00:10 for subtasks toocomplete...

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

Sample complete, hit ENTER tooexit...
```

## <a name="next-steps"></a><span data-ttu-id="47672-237">Další kroky</span><span class="sxs-lookup"><span data-stu-id="47672-237">Next steps</span></span>
* <span data-ttu-id="47672-238">Popisuje Hello Microsoft HPC & Azure Batch Team blog [MPI podporu pro systém Linux na Azure Batch][blog_mpi_linux]a obsahuje informace o používání [OpenFOAM] [ openfoam] pomocí služby Batch.</span><span class="sxs-lookup"><span data-stu-id="47672-238">hello Microsoft HPC & Azure Batch Team blog discusses [MPI support for Linux on Azure Batch][blog_mpi_linux], and includes information on using [OpenFOAM][openfoam] with Batch.</span></span> <span data-ttu-id="47672-239">Můžete najít ukázky kódu Pythonu pro hello [příklad OpenFOAM na Githubu][github_mpi].</span><span class="sxs-lookup"><span data-stu-id="47672-239">You can find Python code samples for hello [OpenFOAM example on GitHub][github_mpi].</span></span>
* <span data-ttu-id="47672-240">Zjistěte, jak příliš[vytvořit fondy Linuxových výpočetních uzlů](batch-linux-nodes.md) pro použití v Azure Batch MPI řešení.</span><span class="sxs-lookup"><span data-stu-id="47672-240">Learn how too[create pools of Linux compute nodes](batch-linux-nodes.md) for use in your Azure Batch MPI solutions.</span></span>

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
