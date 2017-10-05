---
title: "Azure Batch úloh selhání událostí | Microsoft Docs"
description: "Referenční informace pro úlohy Batch nezdaří událostí."
services: batch
author: tamram
manager: timlt
ms.assetid: 
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 04/20/2017
ms.author: tamram
ms.openlocfilehash: 08feb4ec34bb1635f8ea744b54a10b677b94ab3e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="task-fail-event"></a><span data-ttu-id="13343-103">Událost selhání úlohy</span><span class="sxs-lookup"><span data-stu-id="13343-103">Task fail event</span></span>

 <span data-ttu-id="13343-104">Tato událost je vygenerované při úloha skončí s chybou.</span><span class="sxs-lookup"><span data-stu-id="13343-104">This event is emitted when a task completes with a failure.</span></span> <span data-ttu-id="13343-105">Aktuálně všechny nenulový ukončovací kódy se považují za selhání.</span><span class="sxs-lookup"><span data-stu-id="13343-105">Currently all nonzero exit codes are considered failures.</span></span> <span data-ttu-id="13343-106">Tato událost bude vygenerované. *kromě* úlohu dokončit událostí a slouží k rozpoznat, kdy se úloha se nezdařila.</span><span class="sxs-lookup"><span data-stu-id="13343-106">This event will be emitted *in addition to* a task complete event and can be used to detect when a task has failed.</span></span>


 <span data-ttu-id="13343-107">Následující příklad ukazuje, těle úlohy nezdaří událostí.</span><span class="sxs-lookup"><span data-stu-id="13343-107">The following example shows the body of a task fail event.</span></span>

```
{
    "jobId": "job-0000000001",
    "id": "task-5",
    "taskType": "User",
    "systemTaskVersion": 0,
    "nodeInfo": {
        "poolId": "pool-001",
        "nodeId": "tvm-257509324_1-20160908t162728z"
    },
    "multiInstanceSettings": {
        "numberOfInstances": 1
    },
    "constraints": {
        "maxTaskRetryCount": 2
    },
    "executionInfo": {
        "startTime": "2016-09-08T16:32:23.799Z",
        "endTime": "2016-09-08T16:34:00.666Z",
        "exitCode": 1,
        "retryCount": 2,
        "requeueCount": 0
    }
}
```

|<span data-ttu-id="13343-108">Název elementu</span><span class="sxs-lookup"><span data-stu-id="13343-108">Element name</span></span>|<span data-ttu-id="13343-109">Typ</span><span class="sxs-lookup"><span data-stu-id="13343-109">Type</span></span>|<span data-ttu-id="13343-110">Poznámky</span><span class="sxs-lookup"><span data-stu-id="13343-110">Notes</span></span>|
|------------------|----------|-----------|
|<span data-ttu-id="13343-111">JobId</span><span class="sxs-lookup"><span data-stu-id="13343-111">jobId</span></span>|<span data-ttu-id="13343-112">Řetězec</span><span class="sxs-lookup"><span data-stu-id="13343-112">String</span></span>|<span data-ttu-id="13343-113">Id úlohy, která obsahuje úlohu.</span><span class="sxs-lookup"><span data-stu-id="13343-113">The id of the job containing the task.</span></span>|
|<span data-ttu-id="13343-114">id</span><span class="sxs-lookup"><span data-stu-id="13343-114">id</span></span>|<span data-ttu-id="13343-115">Řetězec</span><span class="sxs-lookup"><span data-stu-id="13343-115">String</span></span>|<span data-ttu-id="13343-116">Id úkolu.</span><span class="sxs-lookup"><span data-stu-id="13343-116">The id of the task.</span></span>|
|<span data-ttu-id="13343-117">taskType</span><span class="sxs-lookup"><span data-stu-id="13343-117">taskType</span></span>|<span data-ttu-id="13343-118">Řetězec</span><span class="sxs-lookup"><span data-stu-id="13343-118">String</span></span>|<span data-ttu-id="13343-119">Typ úlohy.</span><span class="sxs-lookup"><span data-stu-id="13343-119">The type of the task.</span></span> <span data-ttu-id="13343-120">To může být JobManager oznamující, že je úkol správce nebo uživatel oznamující, že se nejedná o úkolu Správce úloh.</span><span class="sxs-lookup"><span data-stu-id="13343-120">This can either be 'JobManager' indicating it is a job manager task or 'User' indicating it is not a job manager task.</span></span> <span data-ttu-id="13343-121">Tato událost není vygenerované pro spuštění úlohy, uvolnění úloh nebo přípravy úlohy.</span><span class="sxs-lookup"><span data-stu-id="13343-121">This event is not emitted for job preparation tasks, job release tasks or start tasks.</span></span>|
|<span data-ttu-id="13343-122">systemTaskVersion</span><span class="sxs-lookup"><span data-stu-id="13343-122">systemTaskVersion</span></span>|<span data-ttu-id="13343-123">Int32</span><span class="sxs-lookup"><span data-stu-id="13343-123">Int32</span></span>|<span data-ttu-id="13343-124">Toto je Čítač opakovaných pokusů interní na úlohu.</span><span class="sxs-lookup"><span data-stu-id="13343-124">This is the internal retry counter on a task.</span></span> <span data-ttu-id="13343-125">Služba Batch interně může pokus zopakovat úlohu, aby se zohlednily přechodné problémy.</span><span class="sxs-lookup"><span data-stu-id="13343-125">Internally the Batch service can retry a task to account for transient issues.</span></span> <span data-ttu-id="13343-126">Tyto problémy mohou zahrnovat plánování s interními chybami nebo pokusy o obnovení z výpočetních uzlů ve špatném stavu.</span><span class="sxs-lookup"><span data-stu-id="13343-126">These issues can include internal scheduling errors or attempts to recover from compute nodes in a bad state.</span></span>|
|[<span data-ttu-id="13343-127">nodeInfo</span><span class="sxs-lookup"><span data-stu-id="13343-127">nodeInfo</span></span>](#nodeInfo)|<span data-ttu-id="13343-128">Komplexní typ</span><span class="sxs-lookup"><span data-stu-id="13343-128">Complex Type</span></span>|<span data-ttu-id="13343-129">Obsahuje informace o výpočetním uzlu, na kterém byla úloha spuštěna.</span><span class="sxs-lookup"><span data-stu-id="13343-129">Contains information about the compute node on which the task ran.</span></span>|
|[<span data-ttu-id="13343-130">multiInstanceSettings</span><span class="sxs-lookup"><span data-stu-id="13343-130">multiInstanceSettings</span></span>](#multiInstanceSettings)|<span data-ttu-id="13343-131">Komplexní typ</span><span class="sxs-lookup"><span data-stu-id="13343-131">Complex Type</span></span>|<span data-ttu-id="13343-132">Určuje, že úkol je úlohu s více instancemi nutnosti několika výpočetních uzlech.</span><span class="sxs-lookup"><span data-stu-id="13343-132">Specifies that the task is a Multi-Instance Task requiring multiple compute nodes.</span></span>  <span data-ttu-id="13343-133">V tématu [multiInstanceSettings](https://docs.microsoft.com/rest/api/batchservice/get-information-about-a-task) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="13343-133">See [multiInstanceSettings](https://docs.microsoft.com/rest/api/batchservice/get-information-about-a-task) for details.</span></span>|
|[<span data-ttu-id="13343-134">omezení</span><span class="sxs-lookup"><span data-stu-id="13343-134">constraints</span></span>](#constraints)|<span data-ttu-id="13343-135">Komplexní typ</span><span class="sxs-lookup"><span data-stu-id="13343-135">Complex Type</span></span>|<span data-ttu-id="13343-136">Provádění omezení, které se vztahují k tomuto úkolu.</span><span class="sxs-lookup"><span data-stu-id="13343-136">The execution constraints that apply to this task.</span></span>|
|[<span data-ttu-id="13343-137">executionInfo</span><span class="sxs-lookup"><span data-stu-id="13343-137">executionInfo</span></span>](#executionInfo)|<span data-ttu-id="13343-138">Komplexní typ</span><span class="sxs-lookup"><span data-stu-id="13343-138">Complex Type</span></span>|<span data-ttu-id="13343-139">Obsahuje informace o provádění úlohy.</span><span class="sxs-lookup"><span data-stu-id="13343-139">Contains information about the execution of the task.</span></span>|

###  <span data-ttu-id="13343-140"><a name="nodeInfo"></a>nodeInfo</span><span class="sxs-lookup"><span data-stu-id="13343-140"><a name="nodeInfo"></a> nodeInfo</span></span>

|<span data-ttu-id="13343-141">Název elementu</span><span class="sxs-lookup"><span data-stu-id="13343-141">Element name</span></span>|<span data-ttu-id="13343-142">Typ</span><span class="sxs-lookup"><span data-stu-id="13343-142">Type</span></span>|<span data-ttu-id="13343-143">Poznámky</span><span class="sxs-lookup"><span data-stu-id="13343-143">Notes</span></span>|
|------------------|----------|-----------|
|<span data-ttu-id="13343-144">poolId</span><span class="sxs-lookup"><span data-stu-id="13343-144">poolId</span></span>|<span data-ttu-id="13343-145">Řetězec</span><span class="sxs-lookup"><span data-stu-id="13343-145">String</span></span>|<span data-ttu-id="13343-146">Id fondu, ve kterém byla spuštěna úloha.</span><span class="sxs-lookup"><span data-stu-id="13343-146">The id of the pool on which the task ran.</span></span>|
|<span data-ttu-id="13343-147">nodeId</span><span class="sxs-lookup"><span data-stu-id="13343-147">nodeId</span></span>|<span data-ttu-id="13343-148">Řetězec</span><span class="sxs-lookup"><span data-stu-id="13343-148">String</span></span>|<span data-ttu-id="13343-149">Id uzlu, na kterém byla úloha spuštěna.</span><span class="sxs-lookup"><span data-stu-id="13343-149">The id of the node on which the task ran.</span></span>|

###  <span data-ttu-id="13343-150"><a name="multiInstanceSettings"></a>multiInstanceSettings</span><span class="sxs-lookup"><span data-stu-id="13343-150"><a name="multiInstanceSettings"></a> multiInstanceSettings</span></span>

|<span data-ttu-id="13343-151">Název elementu</span><span class="sxs-lookup"><span data-stu-id="13343-151">Element name</span></span>|<span data-ttu-id="13343-152">Typ</span><span class="sxs-lookup"><span data-stu-id="13343-152">Type</span></span>|<span data-ttu-id="13343-153">Poznámky</span><span class="sxs-lookup"><span data-stu-id="13343-153">Notes</span></span>|
|------------------|----------|-----------|
|<span data-ttu-id="13343-154">numberOfInstances</span><span class="sxs-lookup"><span data-stu-id="13343-154">numberOfInstances</span></span>|<span data-ttu-id="13343-155">Int32</span><span class="sxs-lookup"><span data-stu-id="13343-155">Int32</span></span>|<span data-ttu-id="13343-156">Celkový počet výpočetních uzlů požadovaných úkolem</span><span class="sxs-lookup"><span data-stu-id="13343-156">The number of compute nodes required by the task.</span></span>|

###  <span data-ttu-id="13343-157"><a name="constraints"></a>omezení</span><span class="sxs-lookup"><span data-stu-id="13343-157"><a name="constraints"></a> constraints</span></span>

|<span data-ttu-id="13343-158">Název elementu</span><span class="sxs-lookup"><span data-stu-id="13343-158">Element name</span></span>|<span data-ttu-id="13343-159">Typ</span><span class="sxs-lookup"><span data-stu-id="13343-159">Type</span></span>|<span data-ttu-id="13343-160">Poznámky</span><span class="sxs-lookup"><span data-stu-id="13343-160">Notes</span></span>|
|------------------|----------|-----------|
|<span data-ttu-id="13343-161">maxTaskRetryCount</span><span class="sxs-lookup"><span data-stu-id="13343-161">maxTaskRetryCount</span></span>|<span data-ttu-id="13343-162">Int32</span><span class="sxs-lookup"><span data-stu-id="13343-162">Int32</span></span>|<span data-ttu-id="13343-163">Maximální počet, který může být Opakovat úlohu.</span><span class="sxs-lookup"><span data-stu-id="13343-163">The maximum number of times the task may be retried.</span></span> <span data-ttu-id="13343-164">Služba Batch úkol zopakuje, pokud je jeho ukončovací kód nenulové hodnoty.</span><span class="sxs-lookup"><span data-stu-id="13343-164">The Batch service retries a task if its exit code is nonzero.</span></span><br /><br /> <span data-ttu-id="13343-165">Všimněte si, že tato hodnota konkrétně určuje počet opakovaných pokusů.</span><span class="sxs-lookup"><span data-stu-id="13343-165">Note that this value specifically controls the number of retries.</span></span> <span data-ttu-id="13343-166">Služba Batch bude opakujte úlohu jednou a mohou zkuste až toto omezení.</span><span class="sxs-lookup"><span data-stu-id="13343-166">The Batch service will try the task once, and may then retry up to this limit.</span></span> <span data-ttu-id="13343-167">Například pokud je maximální počet opakování je 3, Batch pokusů a úloh až 4 případech (jeden počáteční pokus a 3 opakování).</span><span class="sxs-lookup"><span data-stu-id="13343-167">For example, if the maximum retry count is 3, Batch tries a task up to 4 times (one initial try and 3 retries).</span></span><br /><br /> <span data-ttu-id="13343-168">Pokud je maximální počet opakování 0, služba Batch neopakuje úlohy.</span><span class="sxs-lookup"><span data-stu-id="13343-168">If the maximum retry count is 0, the Batch service does not retry tasks.</span></span><br /><br /> <span data-ttu-id="13343-169">Pokud je maximální počet opakování −1, služba Batch opakuje úlohy bez omezení.</span><span class="sxs-lookup"><span data-stu-id="13343-169">If the maximum retry count is -1, the Batch service retries tasks without limit.</span></span><br /><br /> <span data-ttu-id="13343-170">Výchozí hodnota je 0 (bez opakování).</span><span class="sxs-lookup"><span data-stu-id="13343-170">The default value is 0 (no retries).</span></span>|


###  <span data-ttu-id="13343-171"><a name="executionInfo"></a>executionInfo</span><span class="sxs-lookup"><span data-stu-id="13343-171"><a name="executionInfo"></a> executionInfo</span></span>

|<span data-ttu-id="13343-172">Název elementu</span><span class="sxs-lookup"><span data-stu-id="13343-172">Element name</span></span>|<span data-ttu-id="13343-173">Typ</span><span class="sxs-lookup"><span data-stu-id="13343-173">Type</span></span>|<span data-ttu-id="13343-174">Poznámky</span><span class="sxs-lookup"><span data-stu-id="13343-174">Notes</span></span>|
|------------------|----------|-----------|
|<span data-ttu-id="13343-175">startTime</span><span class="sxs-lookup"><span data-stu-id="13343-175">startTime</span></span>|<span data-ttu-id="13343-176">Data a času</span><span class="sxs-lookup"><span data-stu-id="13343-176">DateTime</span></span>|<span data-ttu-id="13343-177">Čas, kdy úloha spustí systémem.</span><span class="sxs-lookup"><span data-stu-id="13343-177">The time at which the task started running.</span></span> <span data-ttu-id="13343-178">"Spuštěný" odpovídá **systémem** stavu, takže pokud úloha Určuje soubory prostředků nebo balíčky aplikací, pak počáteční čas zobrazí dobu, kdy úloha spustí stahování nebo nasazování těchto.</span><span class="sxs-lookup"><span data-stu-id="13343-178">'Running' corresponds to the **running** state, so if the task specifies resource files or application packages, then the start time reflects the time at which the task started downloading or deploying these.</span></span>  <span data-ttu-id="13343-179">Pokud byl restartován nebo Opakovat úlohu, je to poslední čas, kdy úloha spustí systémem.</span><span class="sxs-lookup"><span data-stu-id="13343-179">If the task has been restarted or retried, this is the most recent time at which the task started running.</span></span>|
|<span data-ttu-id="13343-180">čas ukončení</span><span class="sxs-lookup"><span data-stu-id="13343-180">endTime</span></span>|<span data-ttu-id="13343-181">Data a času</span><span class="sxs-lookup"><span data-stu-id="13343-181">DateTime</span></span>|<span data-ttu-id="13343-182">Čas, kdy je úloha dokončena.</span><span class="sxs-lookup"><span data-stu-id="13343-182">The time at which the task completed.</span></span>|
|<span data-ttu-id="13343-183">exitCode</span><span class="sxs-lookup"><span data-stu-id="13343-183">exitCode</span></span>|<span data-ttu-id="13343-184">Int32</span><span class="sxs-lookup"><span data-stu-id="13343-184">Int32</span></span>|<span data-ttu-id="13343-185">Kód ukončení úlohy.</span><span class="sxs-lookup"><span data-stu-id="13343-185">The exit code of the task.</span></span>|
|<span data-ttu-id="13343-186">retryCount</span><span class="sxs-lookup"><span data-stu-id="13343-186">retryCount</span></span>|<span data-ttu-id="13343-187">Int32</span><span class="sxs-lookup"><span data-stu-id="13343-187">Int32</span></span>|<span data-ttu-id="13343-188">Počet, kolikrát má byla úloha opakovat službou Batch.</span><span class="sxs-lookup"><span data-stu-id="13343-188">The number of times the task has been retried by the Batch service.</span></span> <span data-ttu-id="13343-189">Úlohy se pokus o Pokud ukončí nenulový ukončovací kód, až do zadaného MaxTaskRetryCount.</span><span class="sxs-lookup"><span data-stu-id="13343-189">The task is retried if it exits with a nonzero exit code, up to the specified MaxTaskRetryCount.</span></span>|
|<span data-ttu-id="13343-190">requeueCount</span><span class="sxs-lookup"><span data-stu-id="13343-190">requeueCount</span></span>|<span data-ttu-id="13343-191">Int32</span><span class="sxs-lookup"><span data-stu-id="13343-191">Int32</span></span>|<span data-ttu-id="13343-192">Počet, kolikrát má byla zařazena úkol službou Batch jako výsledek požadavku uživatele.</span><span class="sxs-lookup"><span data-stu-id="13343-192">The number of times the task has been requeued by the Batch service as the result of a user request.</span></span><br /><br /> <span data-ttu-id="13343-193">Pokud uzly odebere uživatele z fondu (nebo změnou velikosti zmenšení fondu) nebo když je úloha zakázaná, může uživatel zadat, že spuštění úlohy v uzlech být zařazena pro provedení.</span><span class="sxs-lookup"><span data-stu-id="13343-193">When the user removes nodes from a pool (by resizing or shrinking the pool) or when the job is being disabled, the user can specify that running tasks on the nodes be requeued for execution.</span></span> <span data-ttu-id="13343-194">Tento počet sleduje počet opakování úlohy byla zařazena. z těchto důvodů.</span><span class="sxs-lookup"><span data-stu-id="13343-194">This count tracks how many times the task has been requeued for these reasons.</span></span>|
