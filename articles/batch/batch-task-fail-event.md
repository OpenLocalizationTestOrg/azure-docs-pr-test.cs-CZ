---
title: "AAA \"události selhání úlohy Azure Batch | Microsoft Docs\""
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
ms.openlocfilehash: e92604671650900072ba27f807501b704329e865
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="task-fail-event"></a><span data-ttu-id="cf658-103">Událost selhání úlohy</span><span class="sxs-lookup"><span data-stu-id="cf658-103">Task fail event</span></span>

 <span data-ttu-id="cf658-104">Tato událost je vygenerované při úloha skončí s chybou.</span><span class="sxs-lookup"><span data-stu-id="cf658-104">This event is emitted when a task completes with a failure.</span></span> <span data-ttu-id="cf658-105">Aktuálně všechny nenulový ukončovací kódy se považují za selhání.</span><span class="sxs-lookup"><span data-stu-id="cf658-105">Currently all nonzero exit codes are considered failures.</span></span> <span data-ttu-id="cf658-106">Tato událost bude vygenerované. *kromě* úlohu dokončit událostí a může být použité toodetect, pokud úloha se nezdařila.</span><span class="sxs-lookup"><span data-stu-id="cf658-106">This event will be emitted *in addition to* a task complete event and can be used toodetect when a task has failed.</span></span>


 <span data-ttu-id="cf658-107">Hello následující příklad ukazuje textu hello úlohy nezdaří událostí.</span><span class="sxs-lookup"><span data-stu-id="cf658-107">hello following example shows hello body of a task fail event.</span></span>

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

|<span data-ttu-id="cf658-108">Název elementu</span><span class="sxs-lookup"><span data-stu-id="cf658-108">Element name</span></span>|<span data-ttu-id="cf658-109">Typ</span><span class="sxs-lookup"><span data-stu-id="cf658-109">Type</span></span>|<span data-ttu-id="cf658-110">Poznámky</span><span class="sxs-lookup"><span data-stu-id="cf658-110">Notes</span></span>|
|------------------|----------|-----------|
|<span data-ttu-id="cf658-111">JobId</span><span class="sxs-lookup"><span data-stu-id="cf658-111">jobId</span></span>|<span data-ttu-id="cf658-112">Řetězec</span><span class="sxs-lookup"><span data-stu-id="cf658-112">String</span></span>|<span data-ttu-id="cf658-113">id Hello hello úlohy, která obsahuje úlohu hello.</span><span class="sxs-lookup"><span data-stu-id="cf658-113">hello id of hello job containing hello task.</span></span>|
|<span data-ttu-id="cf658-114">id</span><span class="sxs-lookup"><span data-stu-id="cf658-114">id</span></span>|<span data-ttu-id="cf658-115">Řetězec</span><span class="sxs-lookup"><span data-stu-id="cf658-115">String</span></span>|<span data-ttu-id="cf658-116">id Hello hello úlohy.</span><span class="sxs-lookup"><span data-stu-id="cf658-116">hello id of hello task.</span></span>|
|<span data-ttu-id="cf658-117">taskType</span><span class="sxs-lookup"><span data-stu-id="cf658-117">taskType</span></span>|<span data-ttu-id="cf658-118">Řetězec</span><span class="sxs-lookup"><span data-stu-id="cf658-118">String</span></span>|<span data-ttu-id="cf658-119">Typ Hello hello úlohy.</span><span class="sxs-lookup"><span data-stu-id="cf658-119">hello type of hello task.</span></span> <span data-ttu-id="cf658-120">To může být JobManager oznamující, že je úkol správce nebo uživatel oznamující, že se nejedná o úkolu Správce úloh.</span><span class="sxs-lookup"><span data-stu-id="cf658-120">This can either be 'JobManager' indicating it is a job manager task or 'User' indicating it is not a job manager task.</span></span> <span data-ttu-id="cf658-121">Tato událost není vygenerované pro spuštění úlohy, uvolnění úloh nebo přípravy úlohy.</span><span class="sxs-lookup"><span data-stu-id="cf658-121">This event is not emitted for job preparation tasks, job release tasks or start tasks.</span></span>|
|<span data-ttu-id="cf658-122">systemTaskVersion</span><span class="sxs-lookup"><span data-stu-id="cf658-122">systemTaskVersion</span></span>|<span data-ttu-id="cf658-123">Int32</span><span class="sxs-lookup"><span data-stu-id="cf658-123">Int32</span></span>|<span data-ttu-id="cf658-124">Toto je interní opakování čítač hello na úlohu.</span><span class="sxs-lookup"><span data-stu-id="cf658-124">This is hello internal retry counter on a task.</span></span> <span data-ttu-id="cf658-125">Služba Batch hello interně může pokus zopakovat tooaccount úloh pro přechodné problémy.</span><span class="sxs-lookup"><span data-stu-id="cf658-125">Internally hello Batch service can retry a task tooaccount for transient issues.</span></span> <span data-ttu-id="cf658-126">Tyto problémy mohou zahrnovat interní plánování chyby nebo pokusy o toorecover z výpočetních uzlů ve špatném stavu.</span><span class="sxs-lookup"><span data-stu-id="cf658-126">These issues can include internal scheduling errors or attempts toorecover from compute nodes in a bad state.</span></span>|
|[<span data-ttu-id="cf658-127">nodeInfo</span><span class="sxs-lookup"><span data-stu-id="cf658-127">nodeInfo</span></span>](#nodeInfo)|<span data-ttu-id="cf658-128">Komplexní typ</span><span class="sxs-lookup"><span data-stu-id="cf658-128">Complex Type</span></span>|<span data-ttu-id="cf658-129">Obsahuje informace o hello výpočetním uzlu, na které hello úloha spustila.</span><span class="sxs-lookup"><span data-stu-id="cf658-129">Contains information about hello compute node on which hello task ran.</span></span>|
|[<span data-ttu-id="cf658-130">multiInstanceSettings</span><span class="sxs-lookup"><span data-stu-id="cf658-130">multiInstanceSettings</span></span>](#multiInstanceSettings)|<span data-ttu-id="cf658-131">Komplexní typ</span><span class="sxs-lookup"><span data-stu-id="cf658-131">Complex Type</span></span>|<span data-ttu-id="cf658-132">Určuje, že tuto úlohu hello je úkol s více instancemi nutnosti několika výpočetních uzlech.</span><span class="sxs-lookup"><span data-stu-id="cf658-132">Specifies that hello task is a Multi-Instance Task requiring multiple compute nodes.</span></span>  <span data-ttu-id="cf658-133">V tématu [multiInstanceSettings](https://docs.microsoft.com/rest/api/batchservice/get-information-about-a-task) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="cf658-133">See [multiInstanceSettings](https://docs.microsoft.com/rest/api/batchservice/get-information-about-a-task) for details.</span></span>|
|[<span data-ttu-id="cf658-134">omezení</span><span class="sxs-lookup"><span data-stu-id="cf658-134">constraints</span></span>](#constraints)|<span data-ttu-id="cf658-135">Komplexní typ</span><span class="sxs-lookup"><span data-stu-id="cf658-135">Complex Type</span></span>|<span data-ttu-id="cf658-136">Hello provádění omezení, která platí toothis úloh.</span><span class="sxs-lookup"><span data-stu-id="cf658-136">hello execution constraints that apply toothis task.</span></span>|
|[<span data-ttu-id="cf658-137">executionInfo</span><span class="sxs-lookup"><span data-stu-id="cf658-137">executionInfo</span></span>](#executionInfo)|<span data-ttu-id="cf658-138">Komplexní typ</span><span class="sxs-lookup"><span data-stu-id="cf658-138">Complex Type</span></span>|<span data-ttu-id="cf658-139">Obsahuje informace o provádění hello hello úlohy.</span><span class="sxs-lookup"><span data-stu-id="cf658-139">Contains information about hello execution of hello task.</span></span>|

###  <span data-ttu-id="cf658-140"><a name="nodeInfo"></a>nodeInfo</span><span class="sxs-lookup"><span data-stu-id="cf658-140"><a name="nodeInfo"></a> nodeInfo</span></span>

|<span data-ttu-id="cf658-141">Název elementu</span><span class="sxs-lookup"><span data-stu-id="cf658-141">Element name</span></span>|<span data-ttu-id="cf658-142">Typ</span><span class="sxs-lookup"><span data-stu-id="cf658-142">Type</span></span>|<span data-ttu-id="cf658-143">Poznámky</span><span class="sxs-lookup"><span data-stu-id="cf658-143">Notes</span></span>|
|------------------|----------|-----------|
|<span data-ttu-id="cf658-144">poolId</span><span class="sxs-lookup"><span data-stu-id="cf658-144">poolId</span></span>|<span data-ttu-id="cf658-145">Řetězec</span><span class="sxs-lookup"><span data-stu-id="cf658-145">String</span></span>|<span data-ttu-id="cf658-146">id Hello hello fond, na které hello úloha spustila.</span><span class="sxs-lookup"><span data-stu-id="cf658-146">hello id of hello pool on which hello task ran.</span></span>|
|<span data-ttu-id="cf658-147">nodeId</span><span class="sxs-lookup"><span data-stu-id="cf658-147">nodeId</span></span>|<span data-ttu-id="cf658-148">Řetězec</span><span class="sxs-lookup"><span data-stu-id="cf658-148">String</span></span>|<span data-ttu-id="cf658-149">id Hello hello uzlu, na které hello úloha spustila.</span><span class="sxs-lookup"><span data-stu-id="cf658-149">hello id of hello node on which hello task ran.</span></span>|

###  <span data-ttu-id="cf658-150"><a name="multiInstanceSettings"></a>multiInstanceSettings</span><span class="sxs-lookup"><span data-stu-id="cf658-150"><a name="multiInstanceSettings"></a> multiInstanceSettings</span></span>

|<span data-ttu-id="cf658-151">Název elementu</span><span class="sxs-lookup"><span data-stu-id="cf658-151">Element name</span></span>|<span data-ttu-id="cf658-152">Typ</span><span class="sxs-lookup"><span data-stu-id="cf658-152">Type</span></span>|<span data-ttu-id="cf658-153">Poznámky</span><span class="sxs-lookup"><span data-stu-id="cf658-153">Notes</span></span>|
|------------------|----------|-----------|
|<span data-ttu-id="cf658-154">numberOfInstances</span><span class="sxs-lookup"><span data-stu-id="cf658-154">numberOfInstances</span></span>|<span data-ttu-id="cf658-155">Int32</span><span class="sxs-lookup"><span data-stu-id="cf658-155">Int32</span></span>|<span data-ttu-id="cf658-156">Hello počet výpočetních uzlů, které vyžadují hello úloh.</span><span class="sxs-lookup"><span data-stu-id="cf658-156">hello number of compute nodes required by hello task.</span></span>|

###  <span data-ttu-id="cf658-157"><a name="constraints"></a>omezení</span><span class="sxs-lookup"><span data-stu-id="cf658-157"><a name="constraints"></a> constraints</span></span>

|<span data-ttu-id="cf658-158">Název elementu</span><span class="sxs-lookup"><span data-stu-id="cf658-158">Element name</span></span>|<span data-ttu-id="cf658-159">Typ</span><span class="sxs-lookup"><span data-stu-id="cf658-159">Type</span></span>|<span data-ttu-id="cf658-160">Poznámky</span><span class="sxs-lookup"><span data-stu-id="cf658-160">Notes</span></span>|
|------------------|----------|-----------|
|<span data-ttu-id="cf658-161">maxTaskRetryCount</span><span class="sxs-lookup"><span data-stu-id="cf658-161">maxTaskRetryCount</span></span>|<span data-ttu-id="cf658-162">Int32</span><span class="sxs-lookup"><span data-stu-id="cf658-162">Int32</span></span>|<span data-ttu-id="cf658-163">Hello maximální počet opakovaných úkolů hello.</span><span class="sxs-lookup"><span data-stu-id="cf658-163">hello maximum number of times hello task may be retried.</span></span> <span data-ttu-id="cf658-164">Hello služba Batch úkol zopakuje, pokud je jeho ukončovací kód nenulové hodnoty.</span><span class="sxs-lookup"><span data-stu-id="cf658-164">hello Batch service retries a task if its exit code is nonzero.</span></span><br /><br /> <span data-ttu-id="cf658-165">Všimněte si, že tato hodnota řídí konkrétně hello počet opakování.</span><span class="sxs-lookup"><span data-stu-id="cf658-165">Note that this value specifically controls hello number of retries.</span></span> <span data-ttu-id="cf658-166">Služba Batch Hello se pokusí hello úloh jednou a mohou zkuste si toothis limit.</span><span class="sxs-lookup"><span data-stu-id="cf658-166">hello Batch service will try hello task once, and may then retry up toothis limit.</span></span> <span data-ttu-id="cf658-167">Například pokud hello maximální počet opakování je 3, Batch pokusí úlohu až too4 dobu (jeden počáteční pokus a 3 opakování).</span><span class="sxs-lookup"><span data-stu-id="cf658-167">For example, if hello maximum retry count is 3, Batch tries a task up too4 times (one initial try and 3 retries).</span></span><br /><br /> <span data-ttu-id="cf658-168">Pokud hello maximální počet opakování 0, služba Batch hello neopakuje úlohy.</span><span class="sxs-lookup"><span data-stu-id="cf658-168">If hello maximum retry count is 0, hello Batch service does not retry tasks.</span></span><br /><br /> <span data-ttu-id="cf658-169">Pokud hello maximální počet opakování −1, služba Batch hello opakuje úlohy bez omezení.</span><span class="sxs-lookup"><span data-stu-id="cf658-169">If hello maximum retry count is -1, hello Batch service retries tasks without limit.</span></span><br /><br /> <span data-ttu-id="cf658-170">Hello výchozí hodnota je 0 (bez opakování).</span><span class="sxs-lookup"><span data-stu-id="cf658-170">hello default value is 0 (no retries).</span></span>|


###  <span data-ttu-id="cf658-171"><a name="executionInfo"></a>executionInfo</span><span class="sxs-lookup"><span data-stu-id="cf658-171"><a name="executionInfo"></a> executionInfo</span></span>

|<span data-ttu-id="cf658-172">Název elementu</span><span class="sxs-lookup"><span data-stu-id="cf658-172">Element name</span></span>|<span data-ttu-id="cf658-173">Typ</span><span class="sxs-lookup"><span data-stu-id="cf658-173">Type</span></span>|<span data-ttu-id="cf658-174">Poznámky</span><span class="sxs-lookup"><span data-stu-id="cf658-174">Notes</span></span>|
|------------------|----------|-----------|
|<span data-ttu-id="cf658-175">startTime</span><span class="sxs-lookup"><span data-stu-id="cf658-175">startTime</span></span>|<span data-ttu-id="cf658-176">Data a času</span><span class="sxs-lookup"><span data-stu-id="cf658-176">DateTime</span></span>|<span data-ttu-id="cf658-177">Hello čas, který úkol hello spuštění.</span><span class="sxs-lookup"><span data-stu-id="cf658-177">hello time at which hello task started running.</span></span> <span data-ttu-id="cf658-178">"Spuštěný" odpovídá toohello **systémem** stavu, takže pokud úloha hello Určuje soubory prostředků nebo balíčky aplikací, pak počáteční čas hello odráží hello čas, který hello úloha spuštěna stahování nebo nasazování těchto.</span><span class="sxs-lookup"><span data-stu-id="cf658-178">'Running' corresponds toohello **running** state, so if hello task specifies resource files or application packages, then hello start time reflects hello time at which hello task started downloading or deploying these.</span></span>  <span data-ttu-id="cf658-179">Pokud úloha hello byl restartován nebo opakovat, je to hello spuštění poslední čas, který úkol hello.</span><span class="sxs-lookup"><span data-stu-id="cf658-179">If hello task has been restarted or retried, this is hello most recent time at which hello task started running.</span></span>|
|<span data-ttu-id="cf658-180">endTime</span><span class="sxs-lookup"><span data-stu-id="cf658-180">endTime</span></span>|<span data-ttu-id="cf658-181">Data a času</span><span class="sxs-lookup"><span data-stu-id="cf658-181">DateTime</span></span>|<span data-ttu-id="cf658-182">Hello čas, který hello úkol dokončit.</span><span class="sxs-lookup"><span data-stu-id="cf658-182">hello time at which hello task completed.</span></span>|
|<span data-ttu-id="cf658-183">exitCode</span><span class="sxs-lookup"><span data-stu-id="cf658-183">exitCode</span></span>|<span data-ttu-id="cf658-184">Int32</span><span class="sxs-lookup"><span data-stu-id="cf658-184">Int32</span></span>|<span data-ttu-id="cf658-185">ukončovací kód Hello hello úlohy.</span><span class="sxs-lookup"><span data-stu-id="cf658-185">hello exit code of hello task.</span></span>|
|<span data-ttu-id="cf658-186">retryCount</span><span class="sxs-lookup"><span data-stu-id="cf658-186">retryCount</span></span>|<span data-ttu-id="cf658-187">Int32</span><span class="sxs-lookup"><span data-stu-id="cf658-187">Int32</span></span>|<span data-ttu-id="cf658-188">Hello počet oznámení, která má byla hello úloha opakovat hello služby Batch.</span><span class="sxs-lookup"><span data-stu-id="cf658-188">hello number of times hello task has been retried by hello Batch service.</span></span> <span data-ttu-id="cf658-189">Hello úlohy se pokus o Pokud ukončí nenulový ukončovací kód, až toohello zadaný MaxTaskRetryCount.</span><span class="sxs-lookup"><span data-stu-id="cf658-189">hello task is retried if it exits with a nonzero exit code, up toohello specified MaxTaskRetryCount.</span></span>|
|<span data-ttu-id="cf658-190">requeueCount</span><span class="sxs-lookup"><span data-stu-id="cf658-190">requeueCount</span></span>|<span data-ttu-id="cf658-191">Int32</span><span class="sxs-lookup"><span data-stu-id="cf658-191">Int32</span></span>|<span data-ttu-id="cf658-192">Hello kolikrát hello úloh má byla zařazena službou Batch hello hello důsledku požadavku uživatele.</span><span class="sxs-lookup"><span data-stu-id="cf658-192">hello number of times hello task has been requeued by hello Batch service as hello result of a user request.</span></span><br /><br /> <span data-ttu-id="cf658-193">Pokud odebere uživatele hello uzly z fondu (nebo změnou velikosti zmenšení fondu hello) nebo když je úloha hello zakázaná, hello uživatele můžete určit, že spuštění úlohy v uzlech hello být zařazena pro provedení.</span><span class="sxs-lookup"><span data-stu-id="cf658-193">When hello user removes nodes from a pool (by resizing or shrinking hello pool) or when hello job is being disabled, hello user can specify that running tasks on hello nodes be requeued for execution.</span></span> <span data-ttu-id="cf658-194">Tento počet sleduje počet opakování úkolů hello byla zařazena. z těchto důvodů.</span><span class="sxs-lookup"><span data-stu-id="cf658-194">This count tracks how many times hello task has been requeued for these reasons.</span></span>|
