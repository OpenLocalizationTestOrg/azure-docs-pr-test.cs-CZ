---
title: "aaaCreate úloh tooprepare úlohy a dokončení úlohy na výpočetních uzlech - Azure Batch | Microsoft Docs"
description: "Použijte úrovní úlohy přípravy úlohy toominimize data přeneste tooAzure Batch výpočetních uzlů a uvolnění úlohy pro vyčištění uzlu na dokončení úlohy."
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
ms.openlocfilehash: fd5fb47ae6700281e63048c49a1241f4e935baba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="run-job-preparation-and-job-release-tasks-on-batch-compute-nodes"></a><span data-ttu-id="0abfe-103">Spuštění úlohy přípravy a uvolnění úloh na Batch výpočetních uzlů</span><span class="sxs-lookup"><span data-stu-id="0abfe-103">Run job preparation and job release tasks on Batch compute nodes</span></span>

 <span data-ttu-id="0abfe-104">Úlohu služby Azure Batch často vyžaduje určitý způsob instalace předtím, než její úkoly spustí a po úlohy údržby, po dokončení jeho úkolů.</span><span class="sxs-lookup"><span data-stu-id="0abfe-104">An Azure Batch job often requires some form of setup before its tasks are executed, and post-job maintenance when its tasks are completed.</span></span> <span data-ttu-id="0abfe-105">Může potřebovat toodownload běžných úkolů vstupní data tooyour výpočetní uzly, nebo odeslání úloh výstupní data tooAzure úložiště po dokončení úlohy hello.</span><span class="sxs-lookup"><span data-stu-id="0abfe-105">You might need toodownload common task input data tooyour compute nodes, or upload task output data tooAzure Storage after hello job completes.</span></span> <span data-ttu-id="0abfe-106">Můžete použít **úlohy přípravy** a **úlohy verze** úloh tooperform těchto operací.</span><span class="sxs-lookup"><span data-stu-id="0abfe-106">You can use **job preparation** and **job release** tasks tooperform these operations.</span></span>

## <a name="what-are-job-preparation-and-release-tasks"></a><span data-ttu-id="0abfe-107">Co jsou úlohy přípravy a uvolnění úlohy?</span><span class="sxs-lookup"><span data-stu-id="0abfe-107">What are job preparation and release tasks?</span></span>
<span data-ttu-id="0abfe-108">Před spuštěním úlohy, hello úkol přípravy úlohy běží na všech výpočetních uzlů naplánované toorun alespoň jeden úkol.</span><span class="sxs-lookup"><span data-stu-id="0abfe-108">Before a job's tasks run, hello job preparation task runs on all compute nodes scheduled toorun at least one task.</span></span> <span data-ttu-id="0abfe-109">Po dokončení úlohy hello hello úkol uvolnění úlohy se spustí na každém uzlu ve fondu hello, která spouští alespoň jeden úkol.</span><span class="sxs-lookup"><span data-stu-id="0abfe-109">Once hello job is completed, hello job release task runs on each node in hello pool that executed at least one task.</span></span> <span data-ttu-id="0abfe-110">Stejně jako u normálních úkoly služby Batch, můžete zadat příkazovém řádku toobe volána, když úkol příprava nebo uvolnění úlohy běží.</span><span class="sxs-lookup"><span data-stu-id="0abfe-110">As with normal Batch tasks, you can specify a command line toobe invoked when a job preparation or release task is run.</span></span>

<span data-ttu-id="0abfe-111">Přípravy a uvolnění úlohy nabízejí známé dávkové úlohy funkce jako stažení souboru ([soubory prostředků][net_job_prep_resourcefiles]), provedení, vlastní proměnné prostředí, maximální dobu provádění, počet opakování a dobu uchovávání souboru se zvýšenými oprávněními.</span><span class="sxs-lookup"><span data-stu-id="0abfe-111">Job preparation and release tasks offer familiar Batch task features such as file download ([resource files][net_job_prep_resourcefiles]), elevated execution, custom environment variables, maximum execution duration, retry count, and file retention time.</span></span>

<span data-ttu-id="0abfe-112">V následujících částech hello, získáte informace jak toouse hello [JobPreparationTask] [ net_job_prep] a [JobReleaseTask] [ net_job_release] v hello nalezeny třídy [Batch .NET] [ api_net] knihovny.</span><span class="sxs-lookup"><span data-stu-id="0abfe-112">In hello following sections, you'll learn how toouse hello [JobPreparationTask][net_job_prep] and [JobReleaseTask][net_job_release] classes found in hello [Batch .NET][api_net] library.</span></span>

> [!TIP]
> <span data-ttu-id="0abfe-113">Úkoly přípravy a uvolnění úlohy jsou užitečné zejména v prostředích "sdílené fondu", ve které fond výpočetních uzlů potrvají mezi spuštění úloh a používá řadu úloh.</span><span class="sxs-lookup"><span data-stu-id="0abfe-113">Job preparation and release tasks are especially helpful in "shared pool" environments, in which a pool of compute nodes persists between job runs and is used by many jobs.</span></span>
> 
> 

## <a name="when-toouse-job-preparation-and-release-tasks"></a><span data-ttu-id="0abfe-114">Když toouse úlohy přípravy a uvolnění úlohy</span><span class="sxs-lookup"><span data-stu-id="0abfe-114">When toouse job preparation and release tasks</span></span>
<span data-ttu-id="0abfe-115">Úloha přípravy a uvolnění úloh jsou vhodné pro hello následující situace:</span><span class="sxs-lookup"><span data-stu-id="0abfe-115">Job preparation and job release tasks are a good fit for hello following situations:</span></span>

<span data-ttu-id="0abfe-116">**Stahování dat běžných úloh**</span><span class="sxs-lookup"><span data-stu-id="0abfe-116">**Download common task data**</span></span>

<span data-ttu-id="0abfe-117">Úlohy batch často vyžadují společnou sadu dat jako vstup pro hello úlohy.</span><span class="sxs-lookup"><span data-stu-id="0abfe-117">Batch jobs often require a common set of data as input for hello job's tasks.</span></span> <span data-ttu-id="0abfe-118">Například v denní výpočtů analýza rizik a trhu data jsou specifické pro úlohy, ale běžné tooall úkoly v úloze hello.</span><span class="sxs-lookup"><span data-stu-id="0abfe-118">For example, in daily risk analysis calculations, market data is job-specific, yet common tooall tasks in hello job.</span></span> <span data-ttu-id="0abfe-119">Tento trhu data, často několika gigabajtů velikost, by měla být stažené tooeach výpočetním uzlu pouze jednou, aby ho můžete používat všechny úlohy, které běží na uzlu hello.</span><span class="sxs-lookup"><span data-stu-id="0abfe-119">This market data, often several gigabytes in size, should be downloaded tooeach compute node only once so that any task that runs on hello node can use it.</span></span> <span data-ttu-id="0abfe-120">Použití **úkol přípravy úlohy** toodownload tento uzel tooeach data před hello provádění úlohy hello je další úlohy.</span><span class="sxs-lookup"><span data-stu-id="0abfe-120">Use a **job preparation task** toodownload this data tooeach node before hello execution of hello job's other tasks.</span></span>

<span data-ttu-id="0abfe-121">**Odstranit výstup úlohy a úlohy**</span><span class="sxs-lookup"><span data-stu-id="0abfe-121">**Delete job and task output**</span></span>

<span data-ttu-id="0abfe-122">V prostředí "sdílené fondu", kde nejsou vyřazení mezi úlohami fondu výpočetních uzlů, může být nutné toodelete data úlohy mezi spustí.</span><span class="sxs-lookup"><span data-stu-id="0abfe-122">In a "shared pool" environment, where a pool's compute nodes are not decommissioned between jobs, you may need toodelete job data between runs.</span></span> <span data-ttu-id="0abfe-123">Může být nutné tooconserve místa na disku na uzly hello nebo splňují zásady zabezpečení vaší organizace.</span><span class="sxs-lookup"><span data-stu-id="0abfe-123">You might need tooconserve disk space on hello nodes, or satisfy your organization's security policies.</span></span> <span data-ttu-id="0abfe-124">Použití **úkol uvolnění úlohy** toodelete data, která se stáhne úkol přípravy úlohy nebo vygenerovaných během provedení úlohy.</span><span class="sxs-lookup"><span data-stu-id="0abfe-124">Use a **job release task** toodelete data that was downloaded by a job preparation task, or generated during task execution.</span></span>

<span data-ttu-id="0abfe-125">**Uchování protokolu**</span><span class="sxs-lookup"><span data-stu-id="0abfe-125">**Log retention**</span></span>

<span data-ttu-id="0abfe-126">Můžete chtít tookeep kopii soubory protokolů, které vaše úlohy generují nebo možná soubory se stavem systému generovaných aplikací se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="0abfe-126">You might want tookeep a copy of log files that your tasks generate, or perhaps crash dump files that can be generated by failed applications.</span></span> <span data-ttu-id="0abfe-127">Použití **úkol uvolnění úlohy** v takových případech toocompress a nahrajte tato data tooan [Azure Storage] [ azure_storage] účtu.</span><span class="sxs-lookup"><span data-stu-id="0abfe-127">Use a **job release task** in such cases toocompress and upload this data tooan [Azure Storage][azure_storage] account.</span></span>

> [!TIP]
> <span data-ttu-id="0abfe-128">Jiný způsob toopersist protokoly a další úlohy a úkolů výstupní data jsou toouse hello [Azure Batch souboru konvence](batch-task-output.md) knihovny.</span><span class="sxs-lookup"><span data-stu-id="0abfe-128">Another way toopersist logs and other job and task output data is toouse hello [Azure Batch File Conventions](batch-task-output.md) library.</span></span>
> 
> 

## <a name="job-preparation-task"></a><span data-ttu-id="0abfe-129">Úkol přípravy úlohy</span><span class="sxs-lookup"><span data-stu-id="0abfe-129">Job preparation task</span></span>
<span data-ttu-id="0abfe-130">Před spuštěním úlohy služba Batch provádí hello úkol přípravy úlohy na každém výpočetním uzlu, který je toorun naplánovanou úlohu.</span><span class="sxs-lookup"><span data-stu-id="0abfe-130">Before execution of a job's tasks, Batch executes hello job preparation task on each compute node that is scheduled toorun a task.</span></span> <span data-ttu-id="0abfe-131">Ve výchozím nastavení služba Batch hello čeká hello úlohy přípravy úlohy toobe dokončit před spuštěním naplánované tooexecute hello úlohy na uzlu hello.</span><span class="sxs-lookup"><span data-stu-id="0abfe-131">By default, hello Batch service waits for hello job preparation task toobe completed before running hello tasks scheduled tooexecute on hello node.</span></span> <span data-ttu-id="0abfe-132">Můžete ale nakonfigurovat hello služby není toowait.</span><span class="sxs-lookup"><span data-stu-id="0abfe-132">However, you can configure hello service not toowait.</span></span> <span data-ttu-id="0abfe-133">Pokud restartování uzlu hello hello úkol přípravy úlohy znovu spustí, ale můžete také zakázat toto chování.</span><span class="sxs-lookup"><span data-stu-id="0abfe-133">If hello node restarts, hello job preparation task runs again, but you can also disable this behavior.</span></span>

<span data-ttu-id="0abfe-134">Hello úkol přípravy úlohy je provést pouze u uzlů, které jsou naplánované toorun úlohu.</span><span class="sxs-lookup"><span data-stu-id="0abfe-134">hello job preparation task is executed only on nodes that are scheduled toorun a task.</span></span> <span data-ttu-id="0abfe-135">Tím se zabrání hello nepotřebné provádění přípravy úlohy v případě, že uzel není přiřazen úlohu.</span><span class="sxs-lookup"><span data-stu-id="0abfe-135">This prevents hello unnecessary execution of a preparation task in case a node is not assigned a task.</span></span> <span data-ttu-id="0abfe-136">Tato situace může nastat, pokud je číslo hello úloh pro úlohu menší než hello počet uzlů ve fondu.</span><span class="sxs-lookup"><span data-stu-id="0abfe-136">This can occur when hello number of tasks for a job is less than hello number of nodes in a pool.</span></span> <span data-ttu-id="0abfe-137">Také platí, když [provedení souběžné úlohy](batch-parallel-node-tasks.md) je povoleno, což ponechá některé uzly nečinnosti Pokud počet úkolů hello je nižší než hello celkový počet možných souběžných úkolů.</span><span class="sxs-lookup"><span data-stu-id="0abfe-137">It also applies when [concurrent task execution](batch-parallel-node-tasks.md) is enabled, which leaves some nodes idle if hello task count is lower than hello total possible concurrent tasks.</span></span> <span data-ttu-id="0abfe-138">Spuštěním není hello úkol přípravy úlohy na nečinných uzlů, strávíte nižší cenu na poplatky za přenos dat.</span><span class="sxs-lookup"><span data-stu-id="0abfe-138">By not running hello job preparation task on idle nodes, you can spend less money on data transfer charges.</span></span>

> [!NOTE]
> <span data-ttu-id="0abfe-139">[JobPreparationTask] [ net_job_prep_cloudjob] se liší od [CloudPool.StartTask] [ pool_starttask] v tom, že JobPreparationTask provede při spuštění hello každé úlohy, zatímco StartTask spustí, pouze když výpočetního uzlu nejprve spojí fond nebo restartování.</span><span class="sxs-lookup"><span data-stu-id="0abfe-139">[JobPreparationTask][net_job_prep_cloudjob] differs from [CloudPool.StartTask][pool_starttask] in that JobPreparationTask executes at hello start of each job, whereas StartTask executes only when a compute node first joins a pool or restarts.</span></span>
> 
> 

## <a name="job-release-task"></a><span data-ttu-id="0abfe-140">Úkol uvolnění úlohy</span><span class="sxs-lookup"><span data-stu-id="0abfe-140">Job release task</span></span>
<span data-ttu-id="0abfe-141">Jakmile úloha je označena jako dokončená, hello úkol uvolnění úlohy se spustí na každém uzlu ve fondu hello, která spouští alespoň jeden úkol.</span><span class="sxs-lookup"><span data-stu-id="0abfe-141">Once a job is marked as completed, hello job release task is executed on each node in hello pool that executed at least one task.</span></span> <span data-ttu-id="0abfe-142">Úlohu můžete označit jako vydáním žádost o ukončení.</span><span class="sxs-lookup"><span data-stu-id="0abfe-142">You mark a job as completed by issuing a terminate request.</span></span> <span data-ttu-id="0abfe-143">potom nastaví hello stav úlohy příliš Hello služba Batch*ukončení*, ukončí všechny aktivní nebo spuštěné úkoly spojené s úlohou hello a spouští hello úkol uvolnění úlohy.</span><span class="sxs-lookup"><span data-stu-id="0abfe-143">hello Batch service then sets hello job state too*terminating*, terminates any active or running tasks associated with hello job, and runs hello job release task.</span></span> <span data-ttu-id="0abfe-144">Úloha Hello pak se posouvá toohello *Dokončit* stavu.</span><span class="sxs-lookup"><span data-stu-id="0abfe-144">hello job then moves toohello *completed* state.</span></span>

> [!NOTE]
> <span data-ttu-id="0abfe-145">Odstranění úlohy také provede hello úkol uvolnění úlohy.</span><span class="sxs-lookup"><span data-stu-id="0abfe-145">Job deletion also executes hello job release task.</span></span> <span data-ttu-id="0abfe-146">Ale pokud úloha už byla ukončena, hello uvolnění úlohy není spuštěn podruhé Pokud hello úlohy je později odstranit.</span><span class="sxs-lookup"><span data-stu-id="0abfe-146">However, if a job has already been terminated, hello release task is not run a second time if hello job is later deleted.</span></span>
> 
> 

## <a name="job-prep-and-release-tasks-with-batch-net"></a><span data-ttu-id="0abfe-147">Příprava úlohy a uvolnění úlohy pomocí rozhraní Batch .NET</span><span class="sxs-lookup"><span data-stu-id="0abfe-147">Job prep and release tasks with Batch .NET</span></span>
<span data-ttu-id="0abfe-148">přiřadit toouse úkol přípravy úlohy [JobPreparationTask] [ net_job_prep] úlohy tooyour objekt [CloudJob.JobPreparationTask] [ net_job_prep_cloudjob] vlastnost .</span><span class="sxs-lookup"><span data-stu-id="0abfe-148">toouse a job preparation task, assign a [JobPreparationTask][net_job_prep] object tooyour job's [CloudJob.JobPreparationTask][net_job_prep_cloudjob] property.</span></span> <span data-ttu-id="0abfe-149">Podobně, inicializovat [JobReleaseTask] [ net_job_release] a přiřaďte ho tooyour úlohy [CloudJob.JobReleaseTask] [ net_job_prep_cloudjob] vlastnost tooset hello pro úkol uvolnění úlohy.</span><span class="sxs-lookup"><span data-stu-id="0abfe-149">Similarly, initialize a [JobReleaseTask][net_job_release] and assign it tooyour job's [CloudJob.JobReleaseTask][net_job_prep_cloudjob] property tooset hello job's release task.</span></span>

<span data-ttu-id="0abfe-150">V tento fragment kódu `myBatchClient` je instance [BatchClient][net_batch_client], a `myPool` je existující fond v rámci hello účtu Batch.</span><span class="sxs-lookup"><span data-stu-id="0abfe-150">In this code snippet, `myBatchClient` is an instance of [BatchClient][net_batch_client], and `myPool` is an existing pool within hello Batch account.</span></span>

```csharp
// Create hello CloudJob for CloudPool "myPool"
CloudJob myJob =
    myBatchClient.JobOperations.CreateJob(
        "JobPrepReleaseSampleJob",
        new PoolInformation() { PoolId = "myPool" });

// Specify hello command lines for hello job preparation and release tasks
string jobPrepCmdLine =
    "cmd /c echo %AZ_BATCH_NODE_ID% > %AZ_BATCH_NODE_SHARED_DIR%\\shared_file.txt";
string jobReleaseCmdLine =
    "cmd /c del %AZ_BATCH_NODE_SHARED_DIR%\\shared_file.txt";

// Assign hello job preparation task toohello job
myJob.JobPreparationTask =
    new JobPreparationTask { CommandLine = jobPrepCmdLine };

// Assign hello job release task toohello job
myJob.JobReleaseTask =
    new JobPreparationTask { CommandLine = jobReleaseCmdLine };

await myJob.CommitAsync();
```

<span data-ttu-id="0abfe-151">Jak už bylo zmíněno dříve, hello uvolnění úlohy se spustí, až se úloha je ukončeno, nebo odstranit.</span><span class="sxs-lookup"><span data-stu-id="0abfe-151">As mentioned earlier, hello release task is executed when a job is terminated or deleted.</span></span> <span data-ttu-id="0abfe-152">Ukončit úlohu s [JobOperations.TerminateJobAsync][net_job_terminate].</span><span class="sxs-lookup"><span data-stu-id="0abfe-152">Terminate a job with [JobOperations.TerminateJobAsync][net_job_terminate].</span></span> <span data-ttu-id="0abfe-153">Odstranit úlohu s [JobOperations.DeleteJobAsync][net_job_delete].</span><span class="sxs-lookup"><span data-stu-id="0abfe-153">Delete a job with [JobOperations.DeleteJobAsync][net_job_delete].</span></span> <span data-ttu-id="0abfe-154">Obvykle ukončit nebo odstranit úlohu po dokončení jeho úkolů, nebo když bylo dosaženo časového limitu, který jste definovali.</span><span class="sxs-lookup"><span data-stu-id="0abfe-154">You typically terminate or delete a job when its tasks are completed, or when a timeout that you've defined has been reached.</span></span>

```csharp
// Terminate hello job toomark it as Completed; this will initiate the
// Job Release Task on any node that executed job tasks. Note that the
// Job Release Task is also executed when a job is deleted, thus you
// need not call Terminate if you typically delete jobs after task completion.
await myBatchClient.JobOperations.TerminateJobAsy("JobPrepReleaseSampleJob");
```

## <a name="code-sample-on-github"></a><span data-ttu-id="0abfe-155">Ukázka kódu na Githubu</span><span class="sxs-lookup"><span data-stu-id="0abfe-155">Code sample on GitHub</span></span>
<span data-ttu-id="0abfe-156">úkoly přípravy a uvolnění úlohy toosee v akci, podívejte se na hello [JobPrepRelease] [ job_prep_release_sample] ukázkového projektu na Githubu.</span><span class="sxs-lookup"><span data-stu-id="0abfe-156">toosee job preparation and release tasks in action, check out hello [JobPrepRelease][job_prep_release_sample] sample project on GitHub.</span></span> <span data-ttu-id="0abfe-157">Tato Konzolová aplikace hello následující:</span><span class="sxs-lookup"><span data-stu-id="0abfe-157">This console application does hello following:</span></span>

1. <span data-ttu-id="0abfe-158">Vytvoří fond s dvěma uzly "malá".</span><span class="sxs-lookup"><span data-stu-id="0abfe-158">Creates a pool with two "small" nodes.</span></span>
2. <span data-ttu-id="0abfe-159">Vytvoří úlohu s přípravy úlohy, verzi a standardní úlohy.</span><span class="sxs-lookup"><span data-stu-id="0abfe-159">Creates a job with job preparation, release, and standard tasks.</span></span>
3. <span data-ttu-id="0abfe-160">Spustí hello úkol přípravy úlohy, které nejprve zapíše hello uzlu ID tooa textový soubor v adresáři "sdílené" uzlu.</span><span class="sxs-lookup"><span data-stu-id="0abfe-160">Runs hello job preparation task, which first writes hello node ID tooa text file in a node's "shared" directory.</span></span>
4. <span data-ttu-id="0abfe-161">Spustí úlohu na každém uzlu, který zapíše jeho toohello ID úloh stejné textového souboru.</span><span class="sxs-lookup"><span data-stu-id="0abfe-161">Runs a task on each node that writes its task ID toohello same text file.</span></span>
5. <span data-ttu-id="0abfe-162">Jakmile jsou všechny úkoly dokončené (nebo je dosaženo časového limitu hello), vytiskne obsah hello každý uzel textového souboru toohello konzoly.</span><span class="sxs-lookup"><span data-stu-id="0abfe-162">Once all tasks are completed (or hello timeout is reached), prints hello contents of each node's text file toohello console.</span></span>
6. <span data-ttu-id="0abfe-163">Po dokončení úlohy hello spustí hello úlohy verze úloh toodelete hello soubor z uzlu hello.</span><span class="sxs-lookup"><span data-stu-id="0abfe-163">When hello job is completed, runs hello job release task toodelete hello file from hello node.</span></span>
7. <span data-ttu-id="0abfe-164">Hello výtisků ukončovací kódy hello přípravy úlohy a uvolnění úlohy pro každý uzel, na kterém provést.</span><span class="sxs-lookup"><span data-stu-id="0abfe-164">Prints hello exit codes of hello job preparation and release tasks for each node on which they executed.</span></span>
8. <span data-ttu-id="0abfe-165">Pozastaví spuštění tooallow potvrzení odstranění úlohy a fondu.</span><span class="sxs-lookup"><span data-stu-id="0abfe-165">Pauses execution tooallow confirmation of job and/or pool deletion.</span></span>

<span data-ttu-id="0abfe-166">Výstup z ukázkové aplikace hello je podobné toohello následující:</span><span class="sxs-lookup"><span data-stu-id="0abfe-166">Output from hello sample application is similar toohello following:</span></span>

```
Attempting toocreate pool: JobPrepReleaseSamplePool
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

Waiting for job JobPrepReleaseSampleJob tooreach state Completed
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

Sample complete, hit ENTER tooexit...
```

> [!NOTE]
> <span data-ttu-id="0abfe-167">Z důvodu toohello proměnné vytvoření počáteční čas a uzlů v rámci nového fondu (některé uzly jsou připravené pro úlohy než ostatní) může se zobrazit odlišný výstup.</span><span class="sxs-lookup"><span data-stu-id="0abfe-167">Due toohello variable creation and start time of nodes in a new pool (some nodes are ready for tasks before others), you may see different output.</span></span> <span data-ttu-id="0abfe-168">Konkrétně protože hello úkoly dokončí rychle, jeden z uzlů hello fondu může spustit všechny hello úlohy.</span><span class="sxs-lookup"><span data-stu-id="0abfe-168">Specifically, because hello tasks complete quickly, one of hello pool's nodes may execute all of hello job's tasks.</span></span> <span data-ttu-id="0abfe-169">Pokud k tomu dojde, si všimnete, že hello Příprava úlohy a uvolnění úlohy nejsou k dispozici pro hello uzel, který spuštěné žádné úlohy.</span><span class="sxs-lookup"><span data-stu-id="0abfe-169">If this occurs, you will notice that hello job prep and release tasks do not exist for hello node that executed no tasks.</span></span>
> 
> 

### <a name="inspect-job-preparation-and-release-tasks-in-hello-azure-portal"></a><span data-ttu-id="0abfe-170">Zkontrolujte úlohu přípravy a uvolnění úlohy v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="0abfe-170">Inspect job preparation and release tasks in hello Azure portal</span></span>
<span data-ttu-id="0abfe-171">Když spustíte hello ukázkovou aplikaci, můžete použít hello [portál Azure] [ portal] tooview hello vlastnosti hello úlohy a její úkoly nebo stahujte hello sdílené textový soubor, který je upraveném hello úlohy.</span><span class="sxs-lookup"><span data-stu-id="0abfe-171">When you run hello sample application, you can use hello [Azure portal][portal] tooview hello properties of hello job and its tasks, or even download hello shared text file that is modified by hello job's tasks.</span></span>

<span data-ttu-id="0abfe-172">Následující snímek obrazovky Hello ukazuje hello **přípravy úlohy okno** v hello portál Azure po spuštění hello ukázkovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="0abfe-172">hello screenshot below shows hello **Preparation tasks blade** in hello Azure portal after a run of hello sample application.</span></span> <span data-ttu-id="0abfe-173">Přejděte toohello *JobPrepReleaseSampleJob* vlastnosti po dokončení úkolů (ale před odstraněním úlohy a fondu) a klikněte na tlačítko **přípravných kroků** nebo **uvolnění úlohy** tooview jejich vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="0abfe-173">Navigate toohello *JobPrepReleaseSampleJob* properties after your tasks have completed (but before deleting your job and pool) and click **Preparation tasks** or **Release tasks** tooview their properties.</span></span>

![Vlastnosti přípravy úlohy na portálu Azure][1]

## <a name="next-steps"></a><span data-ttu-id="0abfe-175">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0abfe-175">Next steps</span></span>
### <a name="application-packages"></a><span data-ttu-id="0abfe-176">Balíčky aplikací</span><span class="sxs-lookup"><span data-stu-id="0abfe-176">Application packages</span></span>
<span data-ttu-id="0abfe-177">V přidání toohello úkol přípravy úlohy, můžete použít také hello [balíčky aplikací](batch-application-packages.md) funkce Batch tooprepare výpočetní uzly pro provedení úlohy.</span><span class="sxs-lookup"><span data-stu-id="0abfe-177">In addition toohello job preparation task, you can also use hello [application packages](batch-application-packages.md) feature of Batch tooprepare compute nodes for task execution.</span></span> <span data-ttu-id="0abfe-178">Tato funkce je užitečná zejména při nasazení aplikací, které nevyžadují systémem instalační program, aplikace, které obsahují mnoho (100 +) souborů nebo aplikace, které vyžadují striktní verzí.</span><span class="sxs-lookup"><span data-stu-id="0abfe-178">This feature is especially useful for deploying applications that do not require running an installer, applications that contain many (100+) files, or applications that require strict version control.</span></span>

### <a name="installing-applications-and-staging-data"></a><span data-ttu-id="0abfe-179">Instalace aplikací a pracovní data</span><span class="sxs-lookup"><span data-stu-id="0abfe-179">Installing applications and staging data</span></span>
<span data-ttu-id="0abfe-180">Tento příspěvek na fóru MSDN obsahuje přehled několik metod pro spuštěné úkoly přípravy uzlů:</span><span class="sxs-lookup"><span data-stu-id="0abfe-180">This MSDN forum post provides an overview of several methods of preparing your nodes for running tasks:</span></span>

<span data-ttu-id="0abfe-181">[Instalace aplikací a pracovní data na Batch výpočetních uzlů][forum_post]</span><span class="sxs-lookup"><span data-stu-id="0abfe-181">[Installing applications and staging data on Batch compute nodes][forum_post]</span></span>

<span data-ttu-id="0abfe-182">Podle některého z členů týmu Azure Batch hello zapisují, popisuje několik technik, které můžete použít toodeploy aplikacím a datům toocompute uzlů.</span><span class="sxs-lookup"><span data-stu-id="0abfe-182">Written by one of hello Azure Batch team members, it discusses several techniques that you can use toodeploy applications and data toocompute nodes.</span></span>

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
