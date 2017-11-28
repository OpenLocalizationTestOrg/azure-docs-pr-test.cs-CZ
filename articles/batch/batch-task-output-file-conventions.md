---
title: "aaaPersist úloh a úkolů výstup tooAzure úložiště s knihovnou hello souboru konvence pro platformu .NET – Azure Batch | Microsoft Docs"
description: "Zjistěte, jak se knihovna toouse konvence souboru Azure Batch pro .NET toopersist úlohy Batch a tooAzure výstup úlohy úložiště a zobrazení hello trvalý výstupu v hello portálu Azure."
services: batch
documentationcenter: .net
author: tamram
manager: timlt
editor: 
ms.assetid: 16e12d0e-958c-46c2-a6b8-7843835d830e
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 06/16/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: cf2ac8632a13d32438c1bdcf11b4b9649de1e2b5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="persist-job-and-task-data-tooazure-storage-with-hello-batch-file-conventions-library-for-net-toopersist"></a><span data-ttu-id="8db86-103">Zachovat úlohy a úkolů data tooAzure úložiště s knihovnou hello konvence souboru Batch pro .NET toopersist</span><span class="sxs-lookup"><span data-stu-id="8db86-103">Persist job and task data tooAzure Storage with hello Batch File Conventions library for .NET toopersist</span></span> 

[!INCLUDE [batch-task-output-include](../../includes/batch-task-output-include.md)]

<span data-ttu-id="8db86-104">Jedním ze způsobů toopersist úloh dat je toouse hello [konvence souboru Azure Batch library pro .NET][nuget_package].</span><span class="sxs-lookup"><span data-stu-id="8db86-104">One way toopersist task data is toouse hello [Azure Batch File Conventions library for .NET][nuget_package].</span></span> <span data-ttu-id="8db86-105">Hello souboru konvence knihovny zjednodušuje proces hello ukládání výstupu úlohy tooAzure datového úložiště a jejich načtení.</span><span class="sxs-lookup"><span data-stu-id="8db86-105">hello File Conventions library simplifies hello process of storing task output data tooAzure Storage and retrieving it.</span></span> <span data-ttu-id="8db86-106">Můžete použít knihovnu hello názvů souboru v kódu úlohy a klienta &mdash; v kódu úlohy pro zachování soubory a v klientovi kód toolist a jejich načtení.</span><span class="sxs-lookup"><span data-stu-id="8db86-106">You can use hello File Conventions library in both task and client code &mdash; in task code for persisting files, and in client code toolist and retrieve them.</span></span> <span data-ttu-id="8db86-107">Kód úloh můžete také použít hello knihovně tooretrieve hello výstup nadřazeného úlohy, například v [závislosti úkolů](batch-task-dependencies.md) scénář.</span><span class="sxs-lookup"><span data-stu-id="8db86-107">Your task code can also use hello library tooretrieve hello output of upstream tasks, such as in a [task dependencies](batch-task-dependencies.md) scenario.</span></span> 

<span data-ttu-id="8db86-108">tooretrieve výstupní soubory s hello souboru konvence knihovny, můžete vyhledat soubory hello pro dané úlohy nebo úkolu je vypsáním podle ID a účel.</span><span class="sxs-lookup"><span data-stu-id="8db86-108">tooretrieve output files with hello File Conventions library, you can locate hello files for a given job or task by listing them by ID and purpose.</span></span> <span data-ttu-id="8db86-109">Nepotřebujete tooknow hello názvy nebo umístění souborů hello.</span><span class="sxs-lookup"><span data-stu-id="8db86-109">You don't need tooknow hello names or locations of hello files.</span></span> <span data-ttu-id="8db86-110">Můžete například použít hello souboru konvence knihovny toolist všechny zprostředkující soubory pro danou úlohu nebo získat soubor preview pro danou úlohu.</span><span class="sxs-lookup"><span data-stu-id="8db86-110">For example, you can use hello File Conventions library toolist all intermediate files for a given task, or get a preview file for a given job.</span></span>

> [!TIP]
> <span data-ttu-id="8db86-111">Počínaje verzí 2017-05-01, hello rozhraní API služby Batch podporuje zachování výstupní data tooAzure úložiště pro úlohy a úkolech Správce úloh, které běží na fondy, které jsou vytvořené pomocí hello konfigurace virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="8db86-111">Starting with version 2017-05-01, hello Batch service API supports persisting output data tooAzure Storage for tasks and job manager tasks that run on pools created with hello virtual machine configuration.</span></span> <span data-ttu-id="8db86-112">Hello rozhraní API služby Batch poskytuje jednoduchý způsob toopersist, výstup z hello kódu, který vytvoří úlohu a slouží jako již alternativní toohello souboru konvence knihovny.</span><span class="sxs-lookup"><span data-stu-id="8db86-112">hello Batch service API provides a simple way toopersist output from within hello code that creates a task and serves as an alternative toohello File Conventions library.</span></span> <span data-ttu-id="8db86-113">Můžete upravit toopersist výstupu Batch klienta aplikace bez nutnosti tooupdate hello aplikace spuštěná vaše úloha.</span><span class="sxs-lookup"><span data-stu-id="8db86-113">You can modify your Batch client applications toopersist output without needing tooupdate hello application that your task is running.</span></span> <span data-ttu-id="8db86-114">Další informace najdete v tématu [zachovat data tooAzure úloh úložiště s hello rozhraní API služby Batch](batch-task-output-files.md).</span><span class="sxs-lookup"><span data-stu-id="8db86-114">For more information, see [Persist task data tooAzure Storage with hello Batch service API](batch-task-output-files.md).</span></span>
> 
> 

## <a name="when-do-i-use-hello-file-conventions-library-toopersist-task-output"></a><span data-ttu-id="8db86-115">Kdy použít výstup úlohy hello souboru konvence knihovny toopersist?</span><span class="sxs-lookup"><span data-stu-id="8db86-115">When do I use hello File Conventions library toopersist task output?</span></span>

<span data-ttu-id="8db86-116">Azure Batch poskytuje více než jedním ze způsobů toopersist výstup úlohy.</span><span class="sxs-lookup"><span data-stu-id="8db86-116">Azure Batch provides more than one way toopersist task output.</span></span> <span data-ttu-id="8db86-117">Hello konvence souboru je nejlepší vhodná toothese scénáře:</span><span class="sxs-lookup"><span data-stu-id="8db86-117">hello File Conventions is best suited toothese scenarios:</span></span>

- <span data-ttu-id="8db86-118">Můžete snadno upravit hello kód aplikace hello, že vaše úloha pracuje toopersist soubory pomocí hello souboru konvence knihovny.</span><span class="sxs-lookup"><span data-stu-id="8db86-118">You can easily modify hello code for hello application that your task is running toopersist files using hello File Conventions library.</span></span>
- <span data-ttu-id="8db86-119">Chcete data tooAzure toostream úložiště při běhu úlohy hello.</span><span class="sxs-lookup"><span data-stu-id="8db86-119">You want toostream data tooAzure Storage while hello task is still running.</span></span>
- <span data-ttu-id="8db86-120">Chcete data toopersist z fondů vytvořené pomocí konfigurace hello cloudové služby nebo konfigurace virtuálního počítače hello.</span><span class="sxs-lookup"><span data-stu-id="8db86-120">You want toopersist data from pools created with either hello cloud service configuration or hello virtual machine configuration.</span></span>
- <span data-ttu-id="8db86-121">Klientské aplikace nebo dalších úloh v hello úlohy toolocate potřeb a stáhněte soubory výstup úlohy podle ID nebo podle účelu.</span><span class="sxs-lookup"><span data-stu-id="8db86-121">Your client application or other tasks in hello job needs toolocate and download task output files by ID or by purpose.</span></span> 
- <span data-ttu-id="8db86-122">Chcete tooview výstup úlohy v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="8db86-122">You want tooview task output in hello Azure portal.</span></span>

<span data-ttu-id="8db86-123">Pokud váš scénář se liší od výše uvedené, může být nutné tooconsider jiný přístup.</span><span class="sxs-lookup"><span data-stu-id="8db86-123">If your scenario differs from those listed above, you may need tooconsider a different approach.</span></span> <span data-ttu-id="8db86-124">Další informace o dalších možnostech pro zachování výstup úlohy najdete v tématu [zachovat úlohy a úkolů výstupní tooAzure úložiště](batch-task-output.md).</span><span class="sxs-lookup"><span data-stu-id="8db86-124">For more information on other options for persisting task output, see [Persist job and task output tooAzure Storage](batch-task-output.md).</span></span> 

## <a name="what-is-hello-batch-file-conventions-standard"></a><span data-ttu-id="8db86-125">Co je hello dávkového souboru konvence standardní?</span><span class="sxs-lookup"><span data-stu-id="8db86-125">What is hello Batch File Conventions standard?</span></span>

<span data-ttu-id="8db86-126">Hello [dávkového souboru konvence standard](https://github.com/Azure/azure-sdk-for-net/tree/vs17Dev/src/SDKs/Batch/Support/FileConventions#conventions) obsahuje schéma pojmenování pro hello cílové kontejnerů a objektů blob cesty toowhich jsou zapsány výstupní soubory.</span><span class="sxs-lookup"><span data-stu-id="8db86-126">hello [Batch File Conventions standard](https://github.com/Azure/azure-sdk-for-net/tree/vs17Dev/src/SDKs/Batch/Support/FileConventions#conventions) provides a naming scheme for hello destination containers and blob paths toowhich your output files are written.</span></span> <span data-ttu-id="8db86-127">Soubory trvalou tooAzure úložiště, které splňovat toohello souboru konvence standardní jsou automaticky dostupné pro zobrazení v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="8db86-127">Files persisted tooAzure Storage that adhere toohello File Conventions standard are automatically available for viewing in hello Azure portal.</span></span> <span data-ttu-id="8db86-128">portál Hello si je vědoma hello zásady vytváření názvů a proto můžete zobrazit soubory, které splňovat tooit.</span><span class="sxs-lookup"><span data-stu-id="8db86-128">hello portal is aware of hello naming convention and so can display files that adhere tooit.</span></span>

<span data-ttu-id="8db86-129">Hello souboru konvence knihovna pro .NET automaticky názvy kontejnerů úložiště a úloh výstupní soubory podle toohello souboru konvence standardní.</span><span class="sxs-lookup"><span data-stu-id="8db86-129">hello File Conventions library for .NET automatically names your storage containers and task output files according toohello File Conventions standard.</span></span> <span data-ttu-id="8db86-130">Knihovna souboru konvence Hello také poskytuje metody tooquery výstupní soubory ve službě Azure Storage podle toojob ID, ID úlohy nebo účel.</span><span class="sxs-lookup"><span data-stu-id="8db86-130">hello File Conventions library also provides methods tooquery output files in Azure Storage according toojob ID, task ID, or purpose.</span></span>   

<span data-ttu-id="8db86-131">Pokud vyvíjíte s jiném jazyce než v rozhraní .NET, můžete implementovat standard souboru konvence hello sami ve vaší aplikaci.</span><span class="sxs-lookup"><span data-stu-id="8db86-131">If you are developing with a language other than .NET, you can implement hello File Conventions standard yourself in your application.</span></span> <span data-ttu-id="8db86-132">Další informace najdete v tématu [o hello dávkového souboru konvence standard](batch-task-output.md#about-the-batch-file-conventions-standard).</span><span class="sxs-lookup"><span data-stu-id="8db86-132">For more information, see [About hello Batch File Conventions standard](batch-task-output.md#about-the-batch-file-conventions-standard).</span></span>

## <a name="link-an-azure-storage-account-tooyour-batch-account"></a><span data-ttu-id="8db86-133">Odkaz tooyour účtu Azure Storage účtu Batch</span><span class="sxs-lookup"><span data-stu-id="8db86-133">Link an Azure Storage account tooyour Batch account</span></span>

<span data-ttu-id="8db86-134">toopersist výstupní data tooAzure úložiště pomocí hello souboru konvence knihovny, je nutné nejprve propojit tooyour účet Azure Storage účtu Batch.</span><span class="sxs-lookup"><span data-stu-id="8db86-134">toopersist output data tooAzure Storage using hello File Conventions library, you must first link an Azure Storage account tooyour Batch account.</span></span> <span data-ttu-id="8db86-135">Pokud jste tak ještě neučinili, propojení tooyour účet úložiště účtu Batch pomocí hello [portál Azure](https://portal.azure.com):</span><span class="sxs-lookup"><span data-stu-id="8db86-135">If you haven't done so already, link a Storage account tooyour Batch account by using hello [Azure portal](https://portal.azure.com):</span></span>

1. <span data-ttu-id="8db86-136">Přejděte tooyour účtu Batch na portálu Azure hello.</span><span class="sxs-lookup"><span data-stu-id="8db86-136">Navigate tooyour Batch account in hello Azure portal.</span></span> 
2. <span data-ttu-id="8db86-137">V části **nastavení**, vyberte **účet úložiště**.</span><span class="sxs-lookup"><span data-stu-id="8db86-137">Under **Settings**, select **Storage Account**.</span></span>
3. <span data-ttu-id="8db86-138">Pokud již nemáte účet úložiště spojené s vaším účtem Batch, klikněte na tlačítko **účet úložiště (None)**.</span><span class="sxs-lookup"><span data-stu-id="8db86-138">If you do not already have a Storage account associated with your Batch account, click **Storage Account (None)**.</span></span>
4. <span data-ttu-id="8db86-139">Vyberte účet úložiště, ze seznamu hello pro vaše předplatné.</span><span class="sxs-lookup"><span data-stu-id="8db86-139">Select a Storage account from hello list for your subscription.</span></span> <span data-ttu-id="8db86-140">Pro nejlepší výkon, použít účet Azure Storage, který je v hello stejné oblasti jako hello účtu Batch, kde běží vaše úkoly.</span><span class="sxs-lookup"><span data-stu-id="8db86-140">For best performance, use an Azure Storage account that is in hello same region as hello Batch account where your tasks are running.</span></span>

## <a name="persist-output-data"></a><span data-ttu-id="8db86-141">Zachovat výstupní data</span><span class="sxs-lookup"><span data-stu-id="8db86-141">Persist output data</span></span>

<span data-ttu-id="8db86-142">toopersist úloh a úkolů výstupní data s knihovnou hello konvence soubor, vytvořit kontejner ve službě Azure Storage a pak uložte kontejneru toohello výstup hello.</span><span class="sxs-lookup"><span data-stu-id="8db86-142">toopersist job and task output data with hello File Conventions library, create a container in Azure Storage, then save hello output toohello container.</span></span> <span data-ttu-id="8db86-143">Použití hello [Klientská knihovna pro úložiště Azure pro .NET](https://www.nuget.org/packages/WindowsAzure.Storage) v vaše úloha kód tooupload hello úloh výstup toohello kontejneru.</span><span class="sxs-lookup"><span data-stu-id="8db86-143">Use hello [Azure Storage client library for .NET](https://www.nuget.org/packages/WindowsAzure.Storage) in your task code tooupload hello task output toohello container.</span></span> 

<span data-ttu-id="8db86-144">Další informace o práci s kontejnery a objekty BLOB ve službě Azure Storage najdete v tématu [Začínáme s Azure Blob storage pomocí rozhraní .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span><span class="sxs-lookup"><span data-stu-id="8db86-144">For more information about working with containers and blobs in Azure Storage, see [Get started with Azure Blob storage using .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span></span>

> [!WARNING]
> <span data-ttu-id="8db86-145">Všechny výstupy úlohy a úkolů zachovat pomocí konvence souboru hello knihovně jsou uloženy v hello stejnému kontejneru.</span><span class="sxs-lookup"><span data-stu-id="8db86-145">All job and task outputs persisted with hello File Conventions library are stored in hello same container.</span></span> <span data-ttu-id="8db86-146">Pokud zkuste velkého počtu úkolů toopersist soubory v hello stejný čas, [úložiště omezení](../storage/common/storage-performance-checklist.md#blobs) může být požadováno.</span><span class="sxs-lookup"><span data-stu-id="8db86-146">If a large number of tasks try toopersist files at hello same time, [storage throttling limits](../storage/common/storage-performance-checklist.md#blobs) may be enforced.</span></span>
> 
> 

### <a name="create-storage-container"></a><span data-ttu-id="8db86-147">Vytvoření kontejneru úložiště</span><span class="sxs-lookup"><span data-stu-id="8db86-147">Create storage container</span></span>

<span data-ttu-id="8db86-148">toopersist úloh výstup tooAzure úložiště, nejprve vytvořte kontejner voláním [CloudJob][net_cloudjob].[ PrepareOutputStorageAsync][net_prepareoutputasync].</span><span class="sxs-lookup"><span data-stu-id="8db86-148">toopersist task output tooAzure Storage, first create a container by calling [CloudJob][net_cloudjob].[PrepareOutputStorageAsync][net_prepareoutputasync].</span></span> <span data-ttu-id="8db86-149">Tato metoda rozšíření přijímá [CloudStorageAccount] [ net_cloudstorageaccount] objekt jako parametr.</span><span class="sxs-lookup"><span data-stu-id="8db86-149">This extension method takes a [CloudStorageAccount][net_cloudstorageaccount] object as a parameter.</span></span> <span data-ttu-id="8db86-150">Vytvoří kontejner s názvem podle toohello souboru konvence standardní, tak, aby jeho obsah je zjistitelný podle hello Azure portal a hello metody načtení, které jsou popsány dále v článku hello.</span><span class="sxs-lookup"><span data-stu-id="8db86-150">It creates a container named according toohello File Conventions standard,so that its contents are discoverable by hello Azure portal and hello retrieval methods discussed later in hello article.</span></span>

<span data-ttu-id="8db86-151">Obvykle umístit hello kód toocreate kontejner v aplikaci klienta &mdash; hello aplikaci, která vytváří vaše fondy, úlohy a úkoly.</span><span class="sxs-lookup"><span data-stu-id="8db86-151">You typically place hello code toocreate a container in your client application &mdash; hello application that creates your pools, jobs, and tasks.</span></span>

```csharp
CloudJob job = batchClient.JobOperations.CreateJob(
    "myJob",
    new PoolInformation { PoolId = "myPool" });

// Create reference toohello linked Azure Storage account
CloudStorageAccount linkedStorageAccount =
    new CloudStorageAccount(myCredentials, true);

// Create hello blob storage container for hello outputs
await job.PrepareOutputStorageAsync(linkedStorageAccount);
```

### <a name="store-task-outputs"></a><span data-ttu-id="8db86-152">Výstupy úložiště úlohy</span><span class="sxs-lookup"><span data-stu-id="8db86-152">Store task outputs</span></span>

<span data-ttu-id="8db86-153">Teď, když jste připravili kontejner ve službě Azure Storage, úlohy můžete uložit výstupní toohello kontejneru pomocí hello [TaskOutputStorage] [ net_taskoutputstorage] nalezena třída v souboru konvence knihovny hello.</span><span class="sxs-lookup"><span data-stu-id="8db86-153">Now that you've prepared a container in Azure Storage, tasks can save output toohello container by using hello [TaskOutputStorage][net_taskoutputstorage] class found in hello File Conventions library.</span></span>

<span data-ttu-id="8db86-154">Ve vašem kódu úloha nejprve vytvořit [TaskOutputStorage] [ net_taskoutputstorage] objekt a potom po dokončení práce hello úloh volání hello [TaskOutputStorage] [ net_taskoutputstorage]. [SaveAsync] [ net_saveasync] toosave metoda jeho výstup tooAzure úložiště.</span><span class="sxs-lookup"><span data-stu-id="8db86-154">In your task code, first create a [TaskOutputStorage][net_taskoutputstorage] object, then when hello task has completed its work, call hello [TaskOutputStorage][net_taskoutputstorage].[SaveAsync][net_saveasync] method toosave its output tooAzure Storage.</span></span>

```csharp
CloudStorageAccount linkedStorageAccount = new CloudStorageAccount(myCredentials);
string jobId = Environment.GetEnvironmentVariable("AZ_BATCH_JOB_ID");
string taskId = Environment.GetEnvironmentVariable("AZ_BATCH_TASK_ID");

TaskOutputStorage taskOutputStorage = new TaskOutputStorage(
    linkedStorageAccount, jobId, taskId);

/* Code tooprocess data and produce output file(s) */

await taskOutputStorage.SaveAsync(TaskOutputKind.TaskOutput, "frame_full_res.jpg");
await taskOutputStorage.SaveAsync(TaskOutputKind.TaskPreview, "frame_low_res.jpg");
```

<span data-ttu-id="8db86-155">Hello `kind` parametr hello [TaskOutputStorage](https://msdn.microsoft.com/library/microsoft.azure.batch.conventions.files.taskoutputstorage.aspx).[ SaveAsync](https://msdn.microsoft.com/library/microsoft.azure.batch.conventions.files.taskoutputstorage.saveasync.aspx) metoda rozděluje hello trvalé soubory.</span><span class="sxs-lookup"><span data-stu-id="8db86-155">hello `kind` parameter of hello [TaskOutputStorage](https://msdn.microsoft.com/library/microsoft.azure.batch.conventions.files.taskoutputstorage.aspx).[SaveAsync](https://msdn.microsoft.com/library/microsoft.azure.batch.conventions.files.taskoutputstorage.saveasync.aspx) method categorizes hello persisted files.</span></span> <span data-ttu-id="8db86-156">Existují čtyři předdefinované [TaskOutputKind] [ net_taskoutputkind] typy: `TaskOutput`, `TaskPreview`, `TaskLog`, a `TaskIntermediate.` je možné definovat také vlastní kategorie výstupu.</span><span class="sxs-lookup"><span data-stu-id="8db86-156">There are four predefined [TaskOutputKind][net_taskoutputkind] types: `TaskOutput`, `TaskPreview`, `TaskLog`, and `TaskIntermediate.` You can also define custom categories of output.</span></span>

<span data-ttu-id="8db86-157">Tyto typy výstup povolit toospecify výstupy danou úlohu jako trvalý, jaký typ výstupy toolist později při dotazu Batch pro hello.</span><span class="sxs-lookup"><span data-stu-id="8db86-157">These output types allow you toospecify which type of outputs toolist when you later query Batch for hello persisted outputs of a given task.</span></span> <span data-ttu-id="8db86-158">Jinými slovy zobrazením hello výstupy úlohy můžete filtrovat seznam hello na jeden z typů výstup hello.</span><span class="sxs-lookup"><span data-stu-id="8db86-158">In other words, when you list hello outputs for a task, you can filter hello list on one of hello output types.</span></span> <span data-ttu-id="8db86-159">Například "mě hello *preview* výstup pro úlohu *109*."</span><span class="sxs-lookup"><span data-stu-id="8db86-159">For example, "Give me hello *preview* output for task *109*."</span></span> <span data-ttu-id="8db86-160">Další informace o výpis a načítání výstupy se zobrazí v [načíst výstup](#retrieve-output) dále v článku hello.</span><span class="sxs-lookup"><span data-stu-id="8db86-160">More on listing and retrieving outputs appears in [Retrieve output](#retrieve-output) later in hello article.</span></span>

> [!TIP]
> <span data-ttu-id="8db86-161">Hello výstupní typ také určuje, kde v hello Azure portal určitého souboru se zobrazí: *TaskOutput*-seřazený podle kategorií soubory se zobrazí v části **úloh výstupní soubory**, a *TaskLog*soubory se zobrazí v části **úkolů protokoly**.</span><span class="sxs-lookup"><span data-stu-id="8db86-161">hello output kind also determines where in hello Azure portal a particular file appears: *TaskOutput*-categorized files appear under **Task output files**, and *TaskLog* files appear under **Task logs**.</span></span>
> 
> 

### <a name="store-job-outputs"></a><span data-ttu-id="8db86-162">Výstupy úlohy úložiště</span><span class="sxs-lookup"><span data-stu-id="8db86-162">Store job outputs</span></span>

<span data-ttu-id="8db86-163">Kromě toho výstupy úlohy toostoring, můžete uložit hello výstupy přidružené k celé úlohy.</span><span class="sxs-lookup"><span data-stu-id="8db86-163">In addition toostoring task outputs, you can store hello outputs associated with an entire job.</span></span> <span data-ttu-id="8db86-164">Například v úloze sloučení hello film vykreslování úlohy můžete zachovat film hello plně se vykresluje jako výstup úlohy.</span><span class="sxs-lookup"><span data-stu-id="8db86-164">For example, in hello merge task of a movie rendering job, you could persist hello fully rendered movie as a job output.</span></span> <span data-ttu-id="8db86-165">Po dokončení úlohy klientské aplikace můžete seznam načíst hello výstupy úlohy hello a nemá nutné tooquery hello jednotlivé úlohy.</span><span class="sxs-lookup"><span data-stu-id="8db86-165">When your job is completed, your client application can list and retrieve hello outputs for hello job, and does not need tooquery hello individual tasks.</span></span>

<span data-ttu-id="8db86-166">Ukládání výstupu úlohy pomocí volání hello [JobOutputStorage][net_joboutputstorage].[ SaveAsync] [ net_joboutputstorage_saveasync] metoda a zadejte hello [JobOutputKind] [ net_joboutputkind] a název souboru:</span><span class="sxs-lookup"><span data-stu-id="8db86-166">Store job output by calling hello [JobOutputStorage][net_joboutputstorage].[SaveAsync][net_joboutputstorage_saveasync] method, and specify hello [JobOutputKind][net_joboutputkind] and filename:</span></span>

```csharp
CloudJob job = new JobOutputStorage(acct, jobId);
JobOutputStorage jobOutputStorage = job.OutputStorage(linkedStorageAccount);

await jobOutputStorage.SaveAsync(JobOutputKind.JobOutput, "mymovie.mp4");
await jobOutputStorage.SaveAsync(JobOutputKind.JobPreview, "mymovie_preview.mp4");
```

<span data-ttu-id="8db86-167">Stejně jako u hello **TaskOutputKind** typ pro výstupy úlohy, můžete použít hello [JobOutputKind] [ net_joboutputkind] toocategorize typ úlohy je trvalé soubory.</span><span class="sxs-lookup"><span data-stu-id="8db86-167">As with hello **TaskOutputKind** type for task outputs, you use hello [JobOutputKind][net_joboutputkind] type toocategorize a job's persisted files.</span></span> <span data-ttu-id="8db86-168">Tento parametr umožňuje toolater dotazu pro určitý typ výstupu (list).</span><span class="sxs-lookup"><span data-stu-id="8db86-168">This parameter allows you toolater query for (list) a specific type of output.</span></span> <span data-ttu-id="8db86-169">Hello **JobOutputKind** typ obsahuje výstup a preview kategorie a podporuje vytváření vlastních kategorií.</span><span class="sxs-lookup"><span data-stu-id="8db86-169">hello **JobOutputKind** type includes both output and preview categories, and supports creating custom categories.</span></span>

### <a name="store-task-logs"></a><span data-ttu-id="8db86-170">Úloha protokoly úložiště</span><span class="sxs-lookup"><span data-stu-id="8db86-170">Store task logs</span></span>

<span data-ttu-id="8db86-171">Kromě toho toopersisting toodurable úložiště file, pokud úlohy nebo úkolu dokončení, bude pravděpodobně nutné toopersist soubory, které jsou aktualizovány během provádění hello úlohy &mdash; soubory protokolu nebo `stdout.txt` a `stderr.txt`, např.</span><span class="sxs-lookup"><span data-stu-id="8db86-171">In addition toopersisting a file toodurable storage when a task or job completes, you may need toopersist files that are updated during hello execution of a task &mdash; log files or `stdout.txt` and `stderr.txt`, for example.</span></span> <span data-ttu-id="8db86-172">Pro tento účel, poskytuje knihovna Azure Batch souboru konvence hello hello [TaskOutputStorage][net_taskoutputstorage].[ SaveTrackedAsync] [ net_savetrackedasync] metoda.</span><span class="sxs-lookup"><span data-stu-id="8db86-172">For this purpose, hello Azure Batch File Conventions library provides hello [TaskOutputStorage][net_taskoutputstorage].[SaveTrackedAsync][net_savetrackedasync] method.</span></span> <span data-ttu-id="8db86-173">S [SaveTrackedAsync][net_savetrackedasync], můžete sledovat soubor tooa aktualizace v uzlu hello (na časový interval, který zadáte) a zachovat tyto aktualizace tooAzure úložiště.</span><span class="sxs-lookup"><span data-stu-id="8db86-173">With [SaveTrackedAsync][net_savetrackedasync], you can track updates tooa file on hello node (at an interval that you specify) and persist those updates tooAzure Storage.</span></span>

<span data-ttu-id="8db86-174">V hello následující fragment kódu, použijeme [SaveTrackedAsync] [ net_savetrackedasync] tooupdate `stdout.txt` ve službě Azure Storage každých 15 sekund během provádění hello hello úlohy:</span><span class="sxs-lookup"><span data-stu-id="8db86-174">In hello following code snippet, we use [SaveTrackedAsync][net_savetrackedasync] tooupdate `stdout.txt` in Azure Storage every 15 seconds during hello execution of hello task:</span></span>

```csharp
TimeSpan stdoutFlushDelay = TimeSpan.FromSeconds(3);
string logFilePath = Path.Combine(
    Environment.GetEnvironmentVariable("AZ_BATCH_TASK_DIR"), "stdout.txt");

// hello primary task logic is wrapped in a using statement that sends updates to
// hello stdout.txt blob in Storage every 15 seconds while hello task code runs.
using (ITrackedSaveOperation stdout =
        await taskStorage.SaveTrackedAsync(
        TaskOutputKind.TaskLog,
        logFilePath,
        "stdout.txt",
        TimeSpan.FromSeconds(15)))
{
    /* Code tooprocess data and produce output file(s) */

    // We are tracking hello disk file toosave our standard output, but the
    // node agent may take up too3 seconds tooflush hello stdout stream to
    // disk. So give hello file a moment toocatch up.
     await Task.Delay(stdoutFlushDelay);
}
```

<span data-ttu-id="8db86-175">Hello komentář části `Code tooprocess data and produce output file(s)` je zástupný symbol pro hello kód, který by za normálních okolností provedl úlohu.</span><span class="sxs-lookup"><span data-stu-id="8db86-175">hello commented section `Code tooprocess data and produce output file(s)` is a placeholder for hello code that your task would normally perform.</span></span> <span data-ttu-id="8db86-176">Například můžete mít kód, který stahuje data ze služby Azure Storage a provede transformaci nebo výpočet jeho.</span><span class="sxs-lookup"><span data-stu-id="8db86-176">For example, you might have code that downloads data from Azure Storage and performs transformation or calculation on it.</span></span> <span data-ttu-id="8db86-177">Hello důležitou součástí tento fragment kódu je ukázka, jak může obtékat takový kód v `using` bloku tooperiodically aktualizovat soubor s [SaveTrackedAsync][net_savetrackedasync].</span><span class="sxs-lookup"><span data-stu-id="8db86-177">hello important part of this snippet is demonstrating how you can wrap such code in a `using` block tooperiodically update a file with [SaveTrackedAsync][net_savetrackedasync].</span></span>

<span data-ttu-id="8db86-178">agent uzlu Hello je program, který běží na každém uzlu ve fondu hello a poskytuje rozhraní příkazu a řízení hello mezi hello uzlu a služba Batch hello.</span><span class="sxs-lookup"><span data-stu-id="8db86-178">hello node agent is a program that runs on each node in hello pool and provides hello command-and-control interface between hello node and hello Batch service.</span></span> <span data-ttu-id="8db86-179">Hello `Task.Delay` volání je vyžadován na konci hello `using` tooensure bloku, která hello uzlu agenta má čas tooflush hello obsah standard se soubor stdout.txt toohello na uzlu hello.</span><span class="sxs-lookup"><span data-stu-id="8db86-179">hello `Task.Delay` call is required at hello end of this `using` block tooensure that hello node agent has time tooflush hello contents of standard out toohello stdout.txt file on hello node.</span></span> <span data-ttu-id="8db86-180">Bez této zpoždění je možné toomiss hello poslední několik sekund výstupu.</span><span class="sxs-lookup"><span data-stu-id="8db86-180">Without this delay, it is possible toomiss hello last few seconds of output.</span></span> <span data-ttu-id="8db86-181">Tato prodleva nemusí být požadovány pro všechny soubory.</span><span class="sxs-lookup"><span data-stu-id="8db86-181">This delay may not be required for all files.</span></span>

> [!NOTE]
> <span data-ttu-id="8db86-182">Když povolíte sledování pomocí souboru **SaveTrackedAsync**, pouze *připojí* toohello sledovaných souboru jsou trvalé tooAzure úložiště.</span><span class="sxs-lookup"><span data-stu-id="8db86-182">When you enable file tracking with **SaveTrackedAsync**, only *appends* toohello tracked file are persisted tooAzure Storage.</span></span> <span data-ttu-id="8db86-183">Tuto metodu použijte pouze pro sledování-rotaci souborů protokolu nebo jiné soubory, které jsou zapsány toowith připojovat operations toohello konec souboru hello.</span><span class="sxs-lookup"><span data-stu-id="8db86-183">Use this method only for tracking non-rotating log files or other files that are written toowith append operations toohello end of hello file.</span></span>
> 
> 

## <a name="retrieve-output-data"></a><span data-ttu-id="8db86-184">Načíst výstupní data</span><span class="sxs-lookup"><span data-stu-id="8db86-184">Retrieve output data</span></span>

<span data-ttu-id="8db86-185">Můžete načíst pomocí knihovny Azure Batch souboru konvence hello trvalou výstupu, můžete udělat úlohy a úlohy-zaměřená na způsobem.</span><span class="sxs-lookup"><span data-stu-id="8db86-185">When you retrieve your persisted output using hello Azure Batch File Conventions library, you do so in a task- and job-centric manner.</span></span> <span data-ttu-id="8db86-186">Můžete požádat, zda text hello výstup pro daný úloh bez nutnosti tooknow cesty do úložiště Azure, nebo i její název souboru.</span><span class="sxs-lookup"><span data-stu-id="8db86-186">You can request hello output for given task or job without needing tooknow its path in Azure Storage, or even its file name.</span></span> <span data-ttu-id="8db86-187">Místo toho můžete požádat výstupní soubory úlohy nebo úlohy ID.</span><span class="sxs-lookup"><span data-stu-id="8db86-187">Instead, you can request output files by task or job ID.</span></span>

<span data-ttu-id="8db86-188">Hello následující fragment kódu iteruje v rámci úlohy, vytiskne některé informace o hello výstupní soubory pro hello úlohu a potom stáhne jeho soubory z úložiště.</span><span class="sxs-lookup"><span data-stu-id="8db86-188">hello following code snippet iterates through a job's tasks, prints some information about hello output files for hello task, and then downloads its files from Storage.</span></span>

```csharp
foreach (CloudTask task in myJob.ListTasks())
{
    foreach (OutputFileReference output in
        task.OutputStorage(storageAccount).ListOutputs(
            TaskOutputKind.TaskOutput))
    {
        Console.WriteLine($"output file: {output.FilePath}");

        output.DownloadToFileAsync(
            $"{jobId}-{output.FilePath}",
            System.IO.FileMode.Create).Wait();
    }
}
```

## <a name="view-output-files-in-hello-azure-portal"></a><span data-ttu-id="8db86-189">Zobrazení výstupní soubory v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="8db86-189">View output files in hello Azure portal</span></span>

<span data-ttu-id="8db86-190">Hello portál Azure zobrazí úloh výstupní soubory a protokoly, které jsou trvalé tooa propojení účtu úložiště Azure pomocí hello [dávkového souboru konvence standard](https://github.com/Azure/azure-sdk-for-net/tree/vs17Dev/src/SDKs/Batch/Support/FileConventions#conventions).</span><span class="sxs-lookup"><span data-stu-id="8db86-190">hello Azure portal displays task output files and logs that are persisted tooa linked Azure Storage account using hello [Batch File Conventions standard](https://github.com/Azure/azure-sdk-for-net/tree/vs17Dev/src/SDKs/Batch/Support/FileConventions#conventions).</span></span> <span data-ttu-id="8db86-191">Můžete implementovat tyto konvence sami v hello jazyk podle vašeho výběru nebo hello souboru konvence knihovny můžete použít v aplikacích .NET.</span><span class="sxs-lookup"><span data-stu-id="8db86-191">You can implement these conventions yourself in hello a language of your choice, or you can use hello File Conventions library in your .NET applications.</span></span>

<span data-ttu-id="8db86-192">zobrazení hello tooenable souborů výstup hello portálu, musí splňovat následující požadavky hello:</span><span class="sxs-lookup"><span data-stu-id="8db86-192">tooenable hello display of your output files in hello portal, you must satisfy hello following requirements:</span></span>

1. <span data-ttu-id="8db86-193">[Propojení účtu Azure Storage](#requirement-linked-storage-account) tooyour účtu Batch.</span><span class="sxs-lookup"><span data-stu-id="8db86-193">[Link an Azure Storage account](#requirement-linked-storage-account) tooyour Batch account.</span></span>
2. <span data-ttu-id="8db86-194">Dodržovat konvence pojmenování toohello předdefinované kontejnery úložiště a souborů při zachování výstupy.</span><span class="sxs-lookup"><span data-stu-id="8db86-194">Adhere toohello predefined naming conventions for Storage containers and files when persisting outputs.</span></span> <span data-ttu-id="8db86-195">Definice hello tyto konvence můžete najít v souboru konvence knihovny hello [README][github_file_conventions_readme].</span><span class="sxs-lookup"><span data-stu-id="8db86-195">You can find hello definition of these conventions in hello File Conventions library [README][github_file_conventions_readme].</span></span> <span data-ttu-id="8db86-196">Pokud používáte hello [Azure Batch souboru konvence] [ nuget_package] toopersist knihovny vaší výstup, vaše soubory jsou nastavené jako trvalé podle toohello souboru konvence standardní.</span><span class="sxs-lookup"><span data-stu-id="8db86-196">If you use hello [Azure Batch File Conventions][nuget_package] library toopersist your output, your files are persisted according toohello File Conventions standard.</span></span>

<span data-ttu-id="8db86-197">výstup úlohy tooview soubory a protokoly ve hello portálu Azure, přejděte toohello úloh, jejíž výstup vás zajímá, pak klikněte buď **uložená výstupní soubory** nebo **uložené protokoly**.</span><span class="sxs-lookup"><span data-stu-id="8db86-197">tooview task output files and logs in hello Azure portal, navigate toohello task whose output you are interested in, then click either **Saved output files** or **Saved logs**.</span></span> <span data-ttu-id="8db86-198">Tento obrázek ukazuje hello **uložená výstupní soubory** pro hello úlohu s ID "007":</span><span class="sxs-lookup"><span data-stu-id="8db86-198">This image shows hello **Saved output files** for hello task with ID "007":</span></span>

<span data-ttu-id="8db86-199">![Výstupy úlohy okno v hello portálu Azure][2]</span><span class="sxs-lookup"><span data-stu-id="8db86-199">![Task outputs blade in hello Azure portal][2]</span></span>

## <a name="code-sample"></a><span data-ttu-id="8db86-200">Ukázka kódu</span><span class="sxs-lookup"><span data-stu-id="8db86-200">Code sample</span></span>

<span data-ttu-id="8db86-201">Hello [PersistOutputs] [ github_persistoutputs] ukázkový projekt je jedním z hello [ukázky kódu Azure Batch] [ github_samples] na Githubu.</span><span class="sxs-lookup"><span data-stu-id="8db86-201">hello [PersistOutputs][github_persistoutputs] sample project is one of hello [Azure Batch code samples][github_samples] on GitHub.</span></span> <span data-ttu-id="8db86-202">Toto řešení sady Visual Studio ukazuje, jak toouse hello Azure Batch souboru konvence knihovny toopersist úloh výstup toodurable úložiště.</span><span class="sxs-lookup"><span data-stu-id="8db86-202">This Visual Studio solution demonstrates how toouse hello Azure Batch File Conventions library toopersist task output toodurable storage.</span></span> <span data-ttu-id="8db86-203">toorun hello ukázkové, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="8db86-203">toorun hello sample, follow these steps:</span></span>

1. <span data-ttu-id="8db86-204">Hello otevřete projekt v **Visual Studio 2015 nebo novější**.</span><span class="sxs-lookup"><span data-stu-id="8db86-204">Open hello project in **Visual Studio 2015 or newer**.</span></span>
2. <span data-ttu-id="8db86-205">Přidat Batch a Storage **přihlašovací údaje účtu** příliš**AccountSettings.settings** v projektu Microsoft.Azure.Batch.Samples.Common hello.</span><span class="sxs-lookup"><span data-stu-id="8db86-205">Add your Batch and Storage **account credentials** too**AccountSettings.settings** in hello Microsoft.Azure.Batch.Samples.Common project.</span></span>
3. <span data-ttu-id="8db86-206">**Sestavení** (ale není spuštěný) hello řešení.</span><span class="sxs-lookup"><span data-stu-id="8db86-206">**Build** (but do not run) hello solution.</span></span> <span data-ttu-id="8db86-207">Pokud se zobrazí výzva, obnovení všech balíčků NuGet.</span><span class="sxs-lookup"><span data-stu-id="8db86-207">Restore any NuGet packages if prompted.</span></span>
4. <span data-ttu-id="8db86-208">Použití hello Azure portálu tooupload [balíčku aplikace](batch-application-packages.md) pro **PersistOutputsTask**.</span><span class="sxs-lookup"><span data-stu-id="8db86-208">Use hello Azure portal tooupload an [application package](batch-application-packages.md) for **PersistOutputsTask**.</span></span> <span data-ttu-id="8db86-209">Zahrnout hello `PersistOutputsTask.exe` a jeho závislá sestavení v hello ZIP balíčku, ID aplikace hello sady příliš "PersistOutputsTask" a aplikace hello verze balíčku příliš "1.0".</span><span class="sxs-lookup"><span data-stu-id="8db86-209">Include hello `PersistOutputsTask.exe` and its dependent assemblies in hello .zip package, set hello application ID too"PersistOutputsTask", and hello application package version too"1.0".</span></span>
5. <span data-ttu-id="8db86-210">**Spustit** (spustit) hello **PersistOutputs** projektu.</span><span class="sxs-lookup"><span data-stu-id="8db86-210">**Start** (run) hello **PersistOutputs** project.</span></span>
6. <span data-ttu-id="8db86-211">Když výzvami toochoose hello trvalost technologie toouse pro spuštěné hello ukázku, zadejte **1** toorun hello ukázku pomocí výstup úlohy hello souboru konvence knihovny toopersist.</span><span class="sxs-lookup"><span data-stu-id="8db86-211">When prompted toochoose hello persistence technology toouse for running hello sample, enter **1** toorun hello sample using hello File Conventions library toopersist task output.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="8db86-212">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8db86-212">Next steps</span></span>

### <a name="get-hello-batch-file-conventions-library-for-net"></a><span data-ttu-id="8db86-213">Získat hello dávkového souboru konvence knihovna pro .NET</span><span class="sxs-lookup"><span data-stu-id="8db86-213">Get hello Batch File Conventions library for .NET</span></span>

<span data-ttu-id="8db86-214">Hello konvence souboru Batch library pro .NET je k dispozici na [NuGet][nuget_package].</span><span class="sxs-lookup"><span data-stu-id="8db86-214">hello Batch File Conventions library for .NET is available on [NuGet][nuget_package].</span></span> <span data-ttu-id="8db86-215">Hello knihovně rozšiřuje hello [CloudJob] [ net_cloudjob] a [CloudTask] [ net_cloudtask] tříd pomocí nové metody.</span><span class="sxs-lookup"><span data-stu-id="8db86-215">hello library extends hello [CloudJob][net_cloudjob] and [CloudTask][net_cloudtask] classes with new methods.</span></span> <span data-ttu-id="8db86-216">Viz také hello [referenční dokumentace](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.conventions.files) hello souboru konvence knihovny.</span><span class="sxs-lookup"><span data-stu-id="8db86-216">Also see hello [reference documentation](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.conventions.files) for hello File Conventions library.</span></span>

<span data-ttu-id="8db86-217">Hello [zdrojový kód] [ github_file_conventions] hello konvence soubor knihovny je k dispozici na Githubu v hello Microsoft Azure SDK pro .NET úložiště.</span><span class="sxs-lookup"><span data-stu-id="8db86-217">hello [source code][github_file_conventions] for hello File Conventions library is available on GitHub in hello Microsoft Azure SDK for .NET repository.</span></span> 

### <a name="explore-other-approaches-for-persisting-output-data"></a><span data-ttu-id="8db86-218">Prozkoumejte jiné postupy zachování výstupních dat</span><span class="sxs-lookup"><span data-stu-id="8db86-218">Explore other approaches for persisting output data</span></span>

- <span data-ttu-id="8db86-219">V tématu [zachovat úlohy a úkolů výstupní tooAzure úložiště](batch-task-output.md) přehled zachování dat úlohy a úlohy.</span><span class="sxs-lookup"><span data-stu-id="8db86-219">See [Persist job and task output tooAzure Storage](batch-task-output.md) for an overview of persisting task and job data.</span></span>
- <span data-ttu-id="8db86-220">V tématu [zachovat data tooAzure úloh úložiště s hello rozhraní API služby Batch](batch-task-output-files.md) toolearn jak toouse hello rozhraní API služby Batch toopersist výstupní data.</span><span class="sxs-lookup"><span data-stu-id="8db86-220">See [Persist task data tooAzure Storage with hello Batch service API](batch-task-output-files.md) toolearn how toouse hello Batch service API toopersist output data.</span></span>

[forum_post]: https://social.msdn.microsoft.com/Forums/en-US/87b19671-1bdf-427a-972c-2af7e5ba82d9/installing-applications-and-staging-data-on-batch-compute-nodes?forum=azurebatch
[github_file_conventions]: https://github.com/Azure/azure-sdk-for-net/tree/AutoRest/src/Batch/FileConventions
[github_file_conventions_readme]: https://github.com/Azure/azure-sdk-for-net/blob/AutoRest/src/Batch/FileConventions/README.md
[github_persistoutputs]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/PersistOutputs
[github_samples]: https://github.com/Azure/azure-batch-samples
[net_batchclient]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_cloudjob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_cloudstorageaccount]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.cloudstorageaccount.aspx
[net_cloudtask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx
[net_fileconventions_readme]: https://github.com/Azure/azure-sdk-for-net/blob/AutoRest/src/Batch/FileConventions/README.md
[net_joboutputkind]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.joboutputkind.aspx
[net_joboutputstorage]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.joboutputstorage.aspx
[net_joboutputstorage_saveasync]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.joboutputstorage.saveasync.aspx
[net_msdn]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[net_prepareoutputasync]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.cloudjobextensions.prepareoutputstorageasync.aspx
[net_saveasync]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.taskoutputstorage.saveasync.aspx
[net_savetrackedasync]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.taskoutputstorage.savetrackedasync.aspx
[net_taskoutputkind]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.taskoutputkind.aspx
[net_taskoutputstorage]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.taskoutputstorage.aspx
[nuget_manager]: https://docs.nuget.org/consume/installing-nuget
[nuget_package]: https://www.nuget.org/packages/Microsoft.Azure.Batch.Conventions.Files
[portal]: https://portal.azure.com
[storage_explorer]: http://storageexplorer.com/

[1]: ./media/batch-task-output/task-output-01.png "Uložená výstupní soubory a uložené protokoly selektory portálu"
[2]: ./media/batch-task-output/task-output-02.png "Výstupy úlohy okno v hello portálu Azure"
