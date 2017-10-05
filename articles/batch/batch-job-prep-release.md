---
title: "Vytvořte úkoly přípravy úlohy a dokončení úlohy na výpočetních uzlech - Azure Batch | Microsoft Docs"
description: "Minimalizovat přenos dat do výpočetních uzlech Azure Batch pomocí úkolů úrovní úlohy přípravy a uvolnění úlohy pro vyčištění uzlu na dokončení úlohy."
services: batch
documentationcenter: .net
author: tamram
manager: timlt
editor: 
ms.assetid: 63d9d4f1-8521-4bbb-b95a-c4cad73692d3
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 02/27/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 6a2525c02ce7bd3969469d2e28a5fccc948f89b1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="run-job-preparation-and-job-release-tasks-on-batch-compute-nodes"></a><span data-ttu-id="f1563-103">Spuštění úlohy přípravy a uvolnění úloh na Batch výpočetních uzlů</span><span class="sxs-lookup"><span data-stu-id="f1563-103">Run job preparation and job release tasks on Batch compute nodes</span></span>

 <span data-ttu-id="f1563-104">Úlohu služby Azure Batch často vyžaduje určitý způsob instalace předtím, než její úkoly spustí a po úlohy údržby, po dokončení jeho úkolů.</span><span class="sxs-lookup"><span data-stu-id="f1563-104">An Azure Batch job often requires some form of setup before its tasks are executed, and post-job maintenance when its tasks are completed.</span></span> <span data-ttu-id="f1563-105">Možná budete muset stáhnout běžných úkolů vstupní data na výpočetní uzly, nebo odeslání úloh výstupní data do úložiště Azure. Po dokončení úlohy.</span><span class="sxs-lookup"><span data-stu-id="f1563-105">You might need to download common task input data to your compute nodes, or upload task output data to Azure Storage after the job completes.</span></span> <span data-ttu-id="f1563-106">Můžete použít **úlohy přípravy** a **úlohy verze** úlohy musíte provést tyto operace.</span><span class="sxs-lookup"><span data-stu-id="f1563-106">You can use **job preparation** and **job release** tasks to perform these operations.</span></span>

## <a name="what-are-job-preparation-and-release-tasks"></a><span data-ttu-id="f1563-107">Co jsou úlohy přípravy a uvolnění úlohy?</span><span class="sxs-lookup"><span data-stu-id="f1563-107">What are job preparation and release tasks?</span></span>
<span data-ttu-id="f1563-108">Před spuštěním úlohy, spustí úkol přípravy úlohy na všech výpočetních uzlech naplánované spuštění alespoň jeden úkol.</span><span class="sxs-lookup"><span data-stu-id="f1563-108">Before a job's tasks run, the job preparation task runs on all compute nodes scheduled to run at least one task.</span></span> <span data-ttu-id="f1563-109">Po dokončení úlohy se spustí úkol uvolnění úlohy na každém uzlu ve fondu, která spouští alespoň jeden úkol.</span><span class="sxs-lookup"><span data-stu-id="f1563-109">Once the job is completed, the job release task runs on each node in the pool that executed at least one task.</span></span> <span data-ttu-id="f1563-110">Stejně jako u normálních úkoly služby Batch, můžete zadat příkazový řádek, který má být vyvolána při spuštění úlohy přípravné nebo verze úlohy.</span><span class="sxs-lookup"><span data-stu-id="f1563-110">As with normal Batch tasks, you can specify a command line to be invoked when a job preparation or release task is run.</span></span>

<span data-ttu-id="f1563-111">Přípravy a uvolnění úlohy nabízejí známé dávkové úlohy funkce jako stažení souboru ([soubory prostředků][net_job_prep_resourcefiles]), provedení, vlastní proměnné prostředí, maximální dobu provádění, počet opakování a dobu uchovávání souboru se zvýšenými oprávněními.</span><span class="sxs-lookup"><span data-stu-id="f1563-111">Job preparation and release tasks offer familiar Batch task features such as file download ([resource files][net_job_prep_resourcefiles]), elevated execution, custom environment variables, maximum execution duration, retry count, and file retention time.</span></span>

<span data-ttu-id="f1563-112">V následujících částech se naučíte používat [JobPreparationTask] [ net_job_prep] a [JobReleaseTask] [ net_job_release] v nalezeny třídy [Batch .NET] [ api_net] knihovny.</span><span class="sxs-lookup"><span data-stu-id="f1563-112">In the following sections, you'll learn how to use the [JobPreparationTask][net_job_prep] and [JobReleaseTask][net_job_release] classes found in the [Batch .NET][api_net] library.</span></span>

> [!TIP]
> <span data-ttu-id="f1563-113">Úkoly přípravy a uvolnění úlohy jsou užitečné zejména v prostředích "sdílené fondu", ve které fond výpočetních uzlů potrvají mezi spuštění úloh a používá řadu úloh.</span><span class="sxs-lookup"><span data-stu-id="f1563-113">Job preparation and release tasks are especially helpful in "shared pool" environments, in which a pool of compute nodes persists between job runs and is used by many jobs.</span></span>
> 
> 

## <a name="when-to-use-job-preparation-and-release-tasks"></a><span data-ttu-id="f1563-114">Kdy použít úlohy přípravy a uvolnění úlohy</span><span class="sxs-lookup"><span data-stu-id="f1563-114">When to use job preparation and release tasks</span></span>
<span data-ttu-id="f1563-115">Úloha přípravy a uvolnění úloh jsou vhodné pro následující případy:</span><span class="sxs-lookup"><span data-stu-id="f1563-115">Job preparation and job release tasks are a good fit for the following situations:</span></span>

<span data-ttu-id="f1563-116">**Stahování dat běžných úloh**</span><span class="sxs-lookup"><span data-stu-id="f1563-116">**Download common task data**</span></span>

<span data-ttu-id="f1563-117">Úlohy batch často vyžadují společnou sadu dat jako vstup pro úkoly úlohy.</span><span class="sxs-lookup"><span data-stu-id="f1563-117">Batch jobs often require a common set of data as input for the job's tasks.</span></span> <span data-ttu-id="f1563-118">Například v denní výpočtů analýza rizik a trhu data jsou specifické pro úlohy, ještě společné pro všechny úlohy pro úlohu.</span><span class="sxs-lookup"><span data-stu-id="f1563-118">For example, in daily risk analysis calculations, market data is job-specific, yet common to all tasks in the job.</span></span> <span data-ttu-id="f1563-119">Tato data trhu, často několika gigabajtů velikost, by měl být stažen na každém výpočetním uzlu pouze jednou, aby ho můžete používat všechny úlohy, které běží na uzlu.</span><span class="sxs-lookup"><span data-stu-id="f1563-119">This market data, often several gigabytes in size, should be downloaded to each compute node only once so that any task that runs on the node can use it.</span></span> <span data-ttu-id="f1563-120">Použití **úkol přípravy úlohy** ke stažení těchto dat do každého uzlu před provedení úlohy je další úlohy.</span><span class="sxs-lookup"><span data-stu-id="f1563-120">Use a **job preparation task** to download this data to each node before the execution of the job's other tasks.</span></span>

<span data-ttu-id="f1563-121">**Odstranit výstup úlohy a úlohy**</span><span class="sxs-lookup"><span data-stu-id="f1563-121">**Delete job and task output**</span></span>

<span data-ttu-id="f1563-122">V prostředí "sdílené fondu", pokud nejsou fondu výpočetních uzlů vyřazení mezi úlohami, můžete odstranit data úlohy mezi spustí.</span><span class="sxs-lookup"><span data-stu-id="f1563-122">In a "shared pool" environment, where a pool's compute nodes are not decommissioned between jobs, you may need to delete job data between runs.</span></span> <span data-ttu-id="f1563-123">Možná budete muset šetří místo na disku na uzly, nebo splňovat zásady zabezpečení vaší organizace.</span><span class="sxs-lookup"><span data-stu-id="f1563-123">You might need to conserve disk space on the nodes, or satisfy your organization's security policies.</span></span> <span data-ttu-id="f1563-124">Použití **úkol uvolnění úlohy** k odstranění dat, který byl stáhne úkol přípravy úlohy nebo vygenerován při provádění úlohy.</span><span class="sxs-lookup"><span data-stu-id="f1563-124">Use a **job release task** to delete data that was downloaded by a job preparation task, or generated during task execution.</span></span>

<span data-ttu-id="f1563-125">**Uchování protokolu**</span><span class="sxs-lookup"><span data-stu-id="f1563-125">**Log retention**</span></span>

<span data-ttu-id="f1563-126">Můžete chtít zachovat kopii soubory protokolů, které vaše úlohy generují nebo možná havárií souborů výpisu paměti, které může být generována aplikací se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="f1563-126">You might want to keep a copy of log files that your tasks generate, or perhaps crash dump files that can be generated by failed applications.</span></span> <span data-ttu-id="f1563-127">Použití **úkol uvolnění úlohy** v takových případech zkomprimovat a odeslat tato data [Azure Storage] [ azure_storage] účtu.</span><span class="sxs-lookup"><span data-stu-id="f1563-127">Use a **job release task** in such cases to compress and upload this data to an [Azure Storage][azure_storage] account.</span></span>

> [!TIP]
> <span data-ttu-id="f1563-128">Dalším způsobem zachování protokoly a další úlohy a úkolů výstupní data, je použít [Azure Batch souboru konvence](batch-task-output.md) knihovny.</span><span class="sxs-lookup"><span data-stu-id="f1563-128">Another way to persist logs and other job and task output data is to use the [Azure Batch File Conventions](batch-task-output.md) library.</span></span>
> 
> 

## <a name="job-preparation-task"></a><span data-ttu-id="f1563-129">Úkol přípravy úlohy</span><span class="sxs-lookup"><span data-stu-id="f1563-129">Job preparation task</span></span>
<span data-ttu-id="f1563-130">Před spuštěním úlohy služba Batch provádí úkol přípravy úlohy na každém výpočetním uzlu, který je naplánováno spuštění úlohy.</span><span class="sxs-lookup"><span data-stu-id="f1563-130">Before execution of a job's tasks, Batch executes the job preparation task on each compute node that is scheduled to run a task.</span></span> <span data-ttu-id="f1563-131">Ve výchozím nastavení čeká služba Batch úkol přípravy úlohy provést před spuštěním úlohy naplánován ke spuštění na uzlu.</span><span class="sxs-lookup"><span data-stu-id="f1563-131">By default, the Batch service waits for the job preparation task to be completed before running the tasks scheduled to execute on the node.</span></span> <span data-ttu-id="f1563-132">Můžete ale nakonfigurovat službu nechcete čekat.</span><span class="sxs-lookup"><span data-stu-id="f1563-132">However, you can configure the service not to wait.</span></span> <span data-ttu-id="f1563-133">Pokud se uzel restartuje, spustí úkol přípravy úlohy znovu, ale můžete také zakázat toto chování.</span><span class="sxs-lookup"><span data-stu-id="f1563-133">If the node restarts, the job preparation task runs again, but you can also disable this behavior.</span></span>

<span data-ttu-id="f1563-134">Úkol přípravy úlohy je provést pouze u uzlů, které jsou naplánovány pro spuštění úlohy.</span><span class="sxs-lookup"><span data-stu-id="f1563-134">The job preparation task is executed only on nodes that are scheduled to run a task.</span></span> <span data-ttu-id="f1563-135">To zabraňuje zbytečným provádění přípravy úlohy v případě, že uzel není přiřazen úlohu.</span><span class="sxs-lookup"><span data-stu-id="f1563-135">This prevents the unnecessary execution of a preparation task in case a node is not assigned a task.</span></span> <span data-ttu-id="f1563-136">Tato situace může nastat, pokud je počet úloh pro úlohu menší než počet uzlů ve fondu.</span><span class="sxs-lookup"><span data-stu-id="f1563-136">This can occur when the number of tasks for a job is less than the number of nodes in a pool.</span></span> <span data-ttu-id="f1563-137">Také platí, když [provedení souběžné úlohy](batch-parallel-node-tasks.md) je povoleno, což ponechá nečinnosti Pokud některé uzly počet úloh je nižší než celkový počet možných souběžných úkolů.</span><span class="sxs-lookup"><span data-stu-id="f1563-137">It also applies when [concurrent task execution](batch-parallel-node-tasks.md) is enabled, which leaves some nodes idle if the task count is lower than the total possible concurrent tasks.</span></span> <span data-ttu-id="f1563-138">Spuštěním není úkol přípravy úlohy na nečinných uzlů, strávíte nižší cenu na poplatky za přenos dat.</span><span class="sxs-lookup"><span data-stu-id="f1563-138">By not running the job preparation task on idle nodes, you can spend less money on data transfer charges.</span></span>

> [!NOTE]
> <span data-ttu-id="f1563-139">[JobPreparationTask] [ net_job_prep_cloudjob] se liší od [CloudPool.StartTask] [ pool_starttask] v tom, že JobPreparationTask provede na začátku každé úlohy, zatímco StartTask se spustí pouze v případě, že výpočetního uzlu nejprve připojí fondu nebo restartování.</span><span class="sxs-lookup"><span data-stu-id="f1563-139">[JobPreparationTask][net_job_prep_cloudjob] differs from [CloudPool.StartTask][pool_starttask] in that JobPreparationTask executes at the start of each job, whereas StartTask executes only when a compute node first joins a pool or restarts.</span></span>
> 
> 

## <a name="job-release-task"></a><span data-ttu-id="f1563-140">Úkol uvolnění úlohy</span><span class="sxs-lookup"><span data-stu-id="f1563-140">Job release task</span></span>
<span data-ttu-id="f1563-141">Jakmile úloha je označena jako dokončená, úkol uvolnění úlohy se spustí na každém uzlu ve fondu, která spouští alespoň jeden úkol.</span><span class="sxs-lookup"><span data-stu-id="f1563-141">Once a job is marked as completed, the job release task is executed on each node in the pool that executed at least one task.</span></span> <span data-ttu-id="f1563-142">Úlohu můžete označit jako vydáním žádost o ukončení.</span><span class="sxs-lookup"><span data-stu-id="f1563-142">You mark a job as completed by issuing a terminate request.</span></span> <span data-ttu-id="f1563-143">Služba Batch potom nastaví stav úlohy na *ukončení*, ukončí všechny aktivní nebo spuštěné úkoly spojené s úlohou a úkol uvolnění úlohy se spustí.</span><span class="sxs-lookup"><span data-stu-id="f1563-143">The Batch service then sets the job state to *terminating*, terminates any active or running tasks associated with the job, and runs the job release task.</span></span> <span data-ttu-id="f1563-144">Úloha je následně přesunuto do *Dokončit* stavu.</span><span class="sxs-lookup"><span data-stu-id="f1563-144">The job then moves to the *completed* state.</span></span>

> [!NOTE]
> <span data-ttu-id="f1563-145">Odstranění úlohy také spustí úkol uvolnění úlohy.</span><span class="sxs-lookup"><span data-stu-id="f1563-145">Job deletion also executes the job release task.</span></span> <span data-ttu-id="f1563-146">Ale pokud úloha už byla ukončena, uvolnění úlohy není spuštěn podruhé Pokud úloha je později odstranit.</span><span class="sxs-lookup"><span data-stu-id="f1563-146">However, if a job has already been terminated, the release task is not run a second time if the job is later deleted.</span></span>
> 
> 

## <a name="job-prep-and-release-tasks-with-batch-net"></a><span data-ttu-id="f1563-147">Příprava úlohy a uvolnění úlohy pomocí rozhraní Batch .NET</span><span class="sxs-lookup"><span data-stu-id="f1563-147">Job prep and release tasks with Batch .NET</span></span>
<span data-ttu-id="f1563-148">Pokud chcete použít úkol přípravy úlohy, přiřazení [JobPreparationTask] [ net_job_prep] objekt, který má vaše úloha [CloudJob.JobPreparationTask] [ net_job_prep_cloudjob] vlastnost.</span><span class="sxs-lookup"><span data-stu-id="f1563-148">To use a job preparation task, assign a [JobPreparationTask][net_job_prep] object to your job's [CloudJob.JobPreparationTask][net_job_prep_cloudjob] property.</span></span> <span data-ttu-id="f1563-149">Podobně, inicializovat [JobReleaseTask] [ net_job_release] a přiřaďte ho do vaší úlohy [CloudJob.JobReleaseTask] [ net_job_prep_cloudjob] vlastnost nastavit úkol uvolnění úlohy.</span><span class="sxs-lookup"><span data-stu-id="f1563-149">Similarly, initialize a [JobReleaseTask][net_job_release] and assign it to your job's [CloudJob.JobReleaseTask][net_job_prep_cloudjob] property to set the job's release task.</span></span>

<span data-ttu-id="f1563-150">V tento fragment kódu `myBatchClient` je instance [BatchClient][net_batch_client], a `myPool` je existující fond v rámci účtu Batch.</span><span class="sxs-lookup"><span data-stu-id="f1563-150">In this code snippet, `myBatchClient` is an instance of [BatchClient][net_batch_client], and `myPool` is an existing pool within the Batch account.</span></span>

```csharp
// Create the CloudJob for CloudPool "myPool"
CloudJob myJob =
    myBatchClient.JobOperations.CreateJob(
        "JobPrepReleaseSampleJob",
        new PoolInformation() { PoolId = "myPool" });

// Specify the command lines for the job preparation and release tasks
string jobPrepCmdLine =
    "cmd /c echo %AZ_BATCH_NODE_ID% > %AZ_BATCH_NODE_SHARED_DIR%\\shared_file.txt";
string jobReleaseCmdLine =
    "cmd /c del %AZ_BATCH_NODE_SHARED_DIR%\\shared_file.txt";

// Assign the job preparation task to the job
myJob.JobPreparationTask =
    new JobPreparationTask { CommandLine = jobPrepCmdLine };

// Assign the job release task to the job
myJob.JobReleaseTask =
    new JobPreparationTask { CommandLine = jobReleaseCmdLine };

await myJob.CommitAsync();
```

<span data-ttu-id="f1563-151">Jak už bylo zmíněno dříve, uvolnění úlohy se spustí, až se úloha je ukončeno, nebo odstranit.</span><span class="sxs-lookup"><span data-stu-id="f1563-151">As mentioned earlier, the release task is executed when a job is terminated or deleted.</span></span> <span data-ttu-id="f1563-152">Ukončit úlohu s [JobOperations.TerminateJobAsync][net_job_terminate].</span><span class="sxs-lookup"><span data-stu-id="f1563-152">Terminate a job with [JobOperations.TerminateJobAsync][net_job_terminate].</span></span> <span data-ttu-id="f1563-153">Odstranit úlohu s [JobOperations.DeleteJobAsync][net_job_delete].</span><span class="sxs-lookup"><span data-stu-id="f1563-153">Delete a job with [JobOperations.DeleteJobAsync][net_job_delete].</span></span> <span data-ttu-id="f1563-154">Obvykle ukončit nebo odstranit úlohu po dokončení jeho úkolů, nebo když bylo dosaženo časového limitu, který jste definovali.</span><span class="sxs-lookup"><span data-stu-id="f1563-154">You typically terminate or delete a job when its tasks are completed, or when a timeout that you've defined has been reached.</span></span>

```csharp
// Terminate the job to mark it as Completed; this will initiate the
// Job Release Task on any node that executed job tasks. Note that the
// Job Release Task is also executed when a job is deleted, thus you
// need not call Terminate if you typically delete jobs after task completion.
await myBatchClient.JobOperations.TerminateJobAsy("JobPrepReleaseSampleJob");
```

## <a name="code-sample-on-github"></a><span data-ttu-id="f1563-155">Ukázka kódu na Githubu</span><span class="sxs-lookup"><span data-stu-id="f1563-155">Code sample on GitHub</span></span>
<span data-ttu-id="f1563-156">Najdete v části úlohy přípravy a uvolnění úlohy v akci, podívejte se [JobPrepRelease] [ job_prep_release_sample] ukázkového projektu na Githubu.</span><span class="sxs-lookup"><span data-stu-id="f1563-156">To see job preparation and release tasks in action, check out the [JobPrepRelease][job_prep_release_sample] sample project on GitHub.</span></span> <span data-ttu-id="f1563-157">Tato Konzolová aplikace provede následující akce:</span><span class="sxs-lookup"><span data-stu-id="f1563-157">This console application does the following:</span></span>

1. <span data-ttu-id="f1563-158">Vytvoří fond s dvěma uzly "malá".</span><span class="sxs-lookup"><span data-stu-id="f1563-158">Creates a pool with two "small" nodes.</span></span>
2. <span data-ttu-id="f1563-159">Vytvoří úlohu s přípravy úlohy, verzi a standardní úlohy.</span><span class="sxs-lookup"><span data-stu-id="f1563-159">Creates a job with job preparation, release, and standard tasks.</span></span>
3. <span data-ttu-id="f1563-160">Spustí úkol přípravy úlohy, které nejprve zapíše ID uzlu do textového souboru v adresáři "sdílené" uzlu.</span><span class="sxs-lookup"><span data-stu-id="f1563-160">Runs the job preparation task, which first writes the node ID to a text file in a node's "shared" directory.</span></span>
4. <span data-ttu-id="f1563-161">Spustí úlohu na každém uzlu, který zapíše jeho ID úkolu do stejné textového souboru.</span><span class="sxs-lookup"><span data-stu-id="f1563-161">Runs a task on each node that writes its task ID to the same text file.</span></span>
5. <span data-ttu-id="f1563-162">Jakmile jsou všechny úkoly dokončené (nebo je dosaženo časového limitu), vytiskne obsah na konzole pro každý uzel textového souboru.</span><span class="sxs-lookup"><span data-stu-id="f1563-162">Once all tasks are completed (or the timeout is reached), prints the contents of each node's text file to the console.</span></span>
6. <span data-ttu-id="f1563-163">Po dokončení úlohy, spustí úkol uvolnění úlohy odstranění souboru z uzlu.</span><span class="sxs-lookup"><span data-stu-id="f1563-163">When the job is completed, runs the job release task to delete the file from the node.</span></span>
7. <span data-ttu-id="f1563-164">Vytiskne ukončovacích kódů úkolů přípravy a uvolnění úlohy pro každý uzel, na kterém provést.</span><span class="sxs-lookup"><span data-stu-id="f1563-164">Prints the exit codes of the job preparation and release tasks for each node on which they executed.</span></span>
8. <span data-ttu-id="f1563-165">Pozastaví spuštění umožňující potvrzení odstranění úlohy a fondu.</span><span class="sxs-lookup"><span data-stu-id="f1563-165">Pauses execution to allow confirmation of job and/or pool deletion.</span></span>

<span data-ttu-id="f1563-166">Výstup z ukázkové aplikace je podobný následujícímu:</span><span class="sxs-lookup"><span data-stu-id="f1563-166">Output from the sample application is similar to the following:</span></span>

```
Attempting to create pool: JobPrepReleaseSamplePool
Created pool JobPrepReleaseSamplePool with 2 small nodes
Checking for existing job JobPrepReleaseSampleJob...
Job JobPrepReleaseSampleJob not found, creating...
Submitting tasks and awaiting completion...
All tasks completed.

Contents of shared\job_prep_and_release.txt on tvm-2434664350_1-20160623t173951z:
-------------------------------------------
tvm-2434664350_1-20160623t173951z tasks:
  task001
  task004
  task005
  task006

Contents of shared\job_prep_and_release.txt on tvm-2434664350_2-20160623t173951z:
-------------------------------------------
tvm-2434664350_2-20160623t173951z tasks:
  task008
  task002
  task003
  task007

Waiting for job JobPrepReleaseSampleJob to reach state Completed
...

tvm-2434664350_1-20160623t173951z:
  Prep task exit code:    0
  Release task exit code: 0

tvm-2434664350_2-20160623t173951z:
  Prep task exit code:    0
  Release task exit code: 0

Delete job? [yes] no
yes
Delete pool? [yes] no
yes

Sample complete, hit ENTER to exit...
```

> [!NOTE]
> <span data-ttu-id="f1563-167">Z důvodu proměnné vytváření a počáteční čas uzly v rámci nového fondu (některé uzly jsou připravené pro úlohy než ostatní) může se zobrazit odlišný výstup.</span><span class="sxs-lookup"><span data-stu-id="f1563-167">Due to the variable creation and start time of nodes in a new pool (some nodes are ready for tasks before others), you may see different output.</span></span> <span data-ttu-id="f1563-168">Konkrétně protože úlohy dokončit rychle, mezi uzly fondu může provést všechny úkoly úlohy.</span><span class="sxs-lookup"><span data-stu-id="f1563-168">Specifically, because the tasks complete quickly, one of the pool's nodes may execute all of the job's tasks.</span></span> <span data-ttu-id="f1563-169">Pokud k tomu dojde, si všimnete, že příprava úlohy a uvolnění úlohy nejsou k dispozici pro uzel, který spuštěné žádné úlohy.</span><span class="sxs-lookup"><span data-stu-id="f1563-169">If this occurs, you will notice that the job prep and release tasks do not exist for the node that executed no tasks.</span></span>
> 
> 

### <a name="inspect-job-preparation-and-release-tasks-in-the-azure-portal"></a><span data-ttu-id="f1563-170">Zkontrolujte přípravy úlohy a úlohy verzi portálu Azure</span><span class="sxs-lookup"><span data-stu-id="f1563-170">Inspect job preparation and release tasks in the Azure portal</span></span>
<span data-ttu-id="f1563-171">Když spustíte ukázkovou aplikaci, můžete použít [portál Azure] [ portal] pro zobrazení vlastností úlohy a její úkoly nebo i stažení sdílené textový soubor, který je upraveném úkoly úlohy.</span><span class="sxs-lookup"><span data-stu-id="f1563-171">When you run the sample application, you can use the [Azure portal][portal] to view the properties of the job and its tasks, or even download the shared text file that is modified by the job's tasks.</span></span>

<span data-ttu-id="f1563-172">Na snímku obrazovky níže ukazuje **přípravy úlohy okno** na portálu Azure po spuštění ukázkové aplikace.</span><span class="sxs-lookup"><span data-stu-id="f1563-172">The screenshot below shows the **Preparation tasks blade** in the Azure portal after a run of the sample application.</span></span> <span data-ttu-id="f1563-173">Přejděte na *JobPrepReleaseSampleJob* vlastnosti po dokončení úkolů (ale před odstraněním úlohy a fondu) a klikněte na tlačítko **přípravných kroků** nebo **uvolnění úlohy** zobrazit jejich vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="f1563-173">Navigate to the *JobPrepReleaseSampleJob* properties after your tasks have completed (but before deleting your job and pool) and click **Preparation tasks** or **Release tasks** to view their properties.</span></span>

![Vlastnosti přípravy úlohy na portálu Azure][1]

## <a name="next-steps"></a><span data-ttu-id="f1563-175">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f1563-175">Next steps</span></span>
### <a name="application-packages"></a><span data-ttu-id="f1563-176">Balíčky aplikací</span><span class="sxs-lookup"><span data-stu-id="f1563-176">Application packages</span></span>
<span data-ttu-id="f1563-177">Kromě úkol přípravy úlohy, můžete také použít [balíčky aplikací](batch-application-packages.md) služby Batch pro přípravu výpočetních uzlů pro provedení úlohy.</span><span class="sxs-lookup"><span data-stu-id="f1563-177">In addition to the job preparation task, you can also use the [application packages](batch-application-packages.md) feature of Batch to prepare compute nodes for task execution.</span></span> <span data-ttu-id="f1563-178">Tato funkce je užitečná zejména při nasazení aplikací, které nevyžadují systémem instalační program, aplikace, které obsahují mnoho (100 +) souborů nebo aplikace, které vyžadují striktní verzí.</span><span class="sxs-lookup"><span data-stu-id="f1563-178">This feature is especially useful for deploying applications that do not require running an installer, applications that contain many (100+) files, or applications that require strict version control.</span></span>

### <a name="installing-applications-and-staging-data"></a><span data-ttu-id="f1563-179">Instalace aplikací a pracovní data</span><span class="sxs-lookup"><span data-stu-id="f1563-179">Installing applications and staging data</span></span>
<span data-ttu-id="f1563-180">Tento příspěvek na fóru MSDN obsahuje přehled několik metod pro spuštěné úkoly přípravy uzlů:</span><span class="sxs-lookup"><span data-stu-id="f1563-180">This MSDN forum post provides an overview of several methods of preparing your nodes for running tasks:</span></span>

<span data-ttu-id="f1563-181">[Instalace aplikací a pracovní data na Batch výpočetních uzlů][forum_post]</span><span class="sxs-lookup"><span data-stu-id="f1563-181">[Installing applications and staging data on Batch compute nodes][forum_post]</span></span>

<span data-ttu-id="f1563-182">Zapsána pomocí některého z členů týmu Azure Batch, popisuje několik technik, které můžete použít k nasazení aplikace a data na výpočetní uzly.</span><span class="sxs-lookup"><span data-stu-id="f1563-182">Written by one of the Azure Batch team members, it discusses several techniques that you can use to deploy applications and data to compute nodes.</span></span>

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_listjobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobs.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[azure_storage]: https://azure.microsoft.com/services/storage/
[portal]: https://portal.azure.com
[job_prep_release_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/JobPrepRelease
[forum_post]: https://social.msdn.microsoft.com/Forums/en-US/87b19671-1bdf-427a-972c-2af7e5ba82d9/installing-applications-and-staging-data-on-batch-compute-nodes?forum=azurebatch
[net_batch_client]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_cloudjob]:https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_job_prep]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobpreparationtask.aspx
[net_job_prep_cloudjob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobpreparationtask.aspx
[net_job_prep_resourcefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobpreparationtask.resourcefiles.aspx
[net_job_delete]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.deletejobasync.aspx
[net_job_terminate]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.terminatejobasync.aspx
[net_job_release]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobreleasetask.aspx
[net_job_release_cloudjob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobreleasetask.aspx
[pool_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.starttask.aspx

[net_list_certs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.certificateoperations.listcertificates.aspx
[net_list_compute_nodes]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.listcomputenodes.aspx
[net_list_job_schedules]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobscheduleoperations.listjobschedules.aspx
[net_list_jobprep_status]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobpreparationandreleasetaskstatus.aspx
[net_list_jobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobs.aspx
[net_list_nodefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listnodefiles.aspx
[net_list_pools]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.listpools.aspx
[net_list_schedule_jobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobscheduleoperations.listjobs.aspx
[net_list_task_files]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.listnodefiles.aspx
[net_list_tasks]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listtasks.aspx

[1]: ./media/batch-job-prep-release/portal-jobprep-01.png
