---
title: "Azure událost dokončení úlohy Batch | Microsoft Docs"
description: "Referenční dokumentace pro událost po dokončení úlohy Batch."
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
ms.openlocfilehash: 015adf7dbc47c29a78df4e4889b2ee1ddcccdd8e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="task-complete-event"></a><span data-ttu-id="1e169-103">Události po dokončení úloh</span><span class="sxs-lookup"><span data-stu-id="1e169-103">Task complete event</span></span>

 <span data-ttu-id="1e169-104">Tato událost je vygenerované po dokončení úlohy, bez ohledu na to, v něm ukončovací kód.</span><span class="sxs-lookup"><span data-stu-id="1e169-104">This event is emitted once a task is completed, regardless of the exit code.</span></span> <span data-ttu-id="1e169-105">Tato událost slouží k určení dobu trvání nějakého úkolu, kde byla úloha spuštěna, a zda byla opakována.</span><span class="sxs-lookup"><span data-stu-id="1e169-105">This event can be used to determine the duration of a task, where the task ran, and whether it was retried.</span></span>


 <span data-ttu-id="1e169-106">Následující příklad ukazuje text událost dokončení úlohy.</span><span class="sxs-lookup"><span data-stu-id="1e169-106">The following example shows the body of a task complete event.</span></span>

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
        "exitCode": 0,
        "retryCount": 0,
        "requeueCount": 0
    }
}
```

|<span data-ttu-id="1e169-107">Název elementu</span><span class="sxs-lookup"><span data-stu-id="1e169-107">Element name</span></span>|<span data-ttu-id="1e169-108">Typ</span><span class="sxs-lookup"><span data-stu-id="1e169-108">Type</span></span>|<span data-ttu-id="1e169-109">Poznámky</span><span class="sxs-lookup"><span data-stu-id="1e169-109">Notes</span></span>|
|------------------|----------|-----------|
|<span data-ttu-id="1e169-110">JobId</span><span class="sxs-lookup"><span data-stu-id="1e169-110">jobId</span></span>|<span data-ttu-id="1e169-111">Řetězec</span><span class="sxs-lookup"><span data-stu-id="1e169-111">String</span></span>|<span data-ttu-id="1e169-112">Id úlohy, která obsahuje úlohu.</span><span class="sxs-lookup"><span data-stu-id="1e169-112">The id of the job containing the task.</span></span>|
|<span data-ttu-id="1e169-113">id</span><span class="sxs-lookup"><span data-stu-id="1e169-113">id</span></span>|<span data-ttu-id="1e169-114">Řetězec</span><span class="sxs-lookup"><span data-stu-id="1e169-114">String</span></span>|<span data-ttu-id="1e169-115">Id úkolu.</span><span class="sxs-lookup"><span data-stu-id="1e169-115">The id of the task.</span></span>|
|<span data-ttu-id="1e169-116">taskType</span><span class="sxs-lookup"><span data-stu-id="1e169-116">taskType</span></span>|<span data-ttu-id="1e169-117">Řetězec</span><span class="sxs-lookup"><span data-stu-id="1e169-117">String</span></span>|<span data-ttu-id="1e169-118">Typ úlohy.</span><span class="sxs-lookup"><span data-stu-id="1e169-118">The type of the task.</span></span> <span data-ttu-id="1e169-119">To může být JobManager oznamující, že je úkol správce nebo uživatel oznamující, že se nejedná o úkolu Správce úloh.</span><span class="sxs-lookup"><span data-stu-id="1e169-119">This can either be 'JobManager' indicating it is a job manager task or 'User' indicating it is not a job manager task.</span></span> <span data-ttu-id="1e169-120">Tato událost není vygenerované pro spuštění úlohy, uvolnění úloh nebo přípravy úlohy.</span><span class="sxs-lookup"><span data-stu-id="1e169-120">This event is not emitted for job preparation tasks, job release tasks or start tasks.</span></span>|
|<span data-ttu-id="1e169-121">systemTaskVersion</span><span class="sxs-lookup"><span data-stu-id="1e169-121">systemTaskVersion</span></span>|<span data-ttu-id="1e169-122">Int32</span><span class="sxs-lookup"><span data-stu-id="1e169-122">Int32</span></span>|<span data-ttu-id="1e169-123">Toto je Čítač opakovaných pokusů interní na úlohu.</span><span class="sxs-lookup"><span data-stu-id="1e169-123">This is the internal retry counter on a task.</span></span> <span data-ttu-id="1e169-124">Služba Batch interně může pokus zopakovat úlohu, aby se zohlednily přechodné problémy.</span><span class="sxs-lookup"><span data-stu-id="1e169-124">Internally the Batch service can retry a task to account for transient issues.</span></span> <span data-ttu-id="1e169-125">Tyto problémy mohou zahrnovat plánování s interními chybami nebo pokusy o obnovení z výpočetních uzlů ve špatném stavu.</span><span class="sxs-lookup"><span data-stu-id="1e169-125">These issues can include internal scheduling errors or attempts to recover from compute nodes in a bad state.</span></span>|
|[<span data-ttu-id="1e169-126">nodeInfo</span><span class="sxs-lookup"><span data-stu-id="1e169-126">nodeInfo</span></span>](#nodeInfo)|<span data-ttu-id="1e169-127">Komplexní typ</span><span class="sxs-lookup"><span data-stu-id="1e169-127">Complex Type</span></span>|<span data-ttu-id="1e169-128">Obsahuje informace o výpočetním uzlu, na kterém byla úloha spuštěna.</span><span class="sxs-lookup"><span data-stu-id="1e169-128">Contains information about the compute node on which the task ran.</span></span>|
|[<span data-ttu-id="1e169-129">multiInstanceSettings</span><span class="sxs-lookup"><span data-stu-id="1e169-129">multiInstanceSettings</span></span>](#multiInstanceSettings)|<span data-ttu-id="1e169-130">Komplexní typ</span><span class="sxs-lookup"><span data-stu-id="1e169-130">Complex Type</span></span>|<span data-ttu-id="1e169-131">Určuje, že úkol je úlohu s více instancemi nutnosti několika výpočetních uzlech.</span><span class="sxs-lookup"><span data-stu-id="1e169-131">Specifies that the task is a Multi-Instance Task requiring multiple compute nodes.</span></span>  <span data-ttu-id="1e169-132">V tématu [multiInstanceSettings](https://docs.microsoft.com/rest/api/batchservice/get-information-about-a-task) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="1e169-132">See [multiInstanceSettings](https://docs.microsoft.com/rest/api/batchservice/get-information-about-a-task) for details.</span></span>|
|[<span data-ttu-id="1e169-133">omezení</span><span class="sxs-lookup"><span data-stu-id="1e169-133">constraints</span></span>](#constraints)|<span data-ttu-id="1e169-134">Komplexní typ</span><span class="sxs-lookup"><span data-stu-id="1e169-134">Complex Type</span></span>|<span data-ttu-id="1e169-135">Provádění omezení, které se vztahují k tomuto úkolu.</span><span class="sxs-lookup"><span data-stu-id="1e169-135">The execution constraints that apply to this task.</span></span>|
|[<span data-ttu-id="1e169-136">executionInfo</span><span class="sxs-lookup"><span data-stu-id="1e169-136">executionInfo</span></span>](#executionInfo)|<span data-ttu-id="1e169-137">Komplexní typ</span><span class="sxs-lookup"><span data-stu-id="1e169-137">Complex Type</span></span>|<span data-ttu-id="1e169-138">Obsahuje informace o provádění úlohy.</span><span class="sxs-lookup"><span data-stu-id="1e169-138">Contains information about the execution of the task.</span></span>|

###  <span data-ttu-id="1e169-139"><a name="nodeInfo"></a>nodeInfo</span><span class="sxs-lookup"><span data-stu-id="1e169-139"><a name="nodeInfo"></a> nodeInfo</span></span>

|<span data-ttu-id="1e169-140">Název elementu</span><span class="sxs-lookup"><span data-stu-id="1e169-140">Element name</span></span>|<span data-ttu-id="1e169-141">Typ</span><span class="sxs-lookup"><span data-stu-id="1e169-141">Type</span></span>|<span data-ttu-id="1e169-142">Poznámky</span><span class="sxs-lookup"><span data-stu-id="1e169-142">Notes</span></span>|
|------------------|----------|-----------|
|<span data-ttu-id="1e169-143">poolId</span><span class="sxs-lookup"><span data-stu-id="1e169-143">poolId</span></span>|<span data-ttu-id="1e169-144">Řetězec</span><span class="sxs-lookup"><span data-stu-id="1e169-144">String</span></span>|<span data-ttu-id="1e169-145">Id fondu, ve kterém byla spuštěna úloha.</span><span class="sxs-lookup"><span data-stu-id="1e169-145">The id of the pool on which the task ran.</span></span>|
|<span data-ttu-id="1e169-146">nodeId</span><span class="sxs-lookup"><span data-stu-id="1e169-146">nodeId</span></span>|<span data-ttu-id="1e169-147">Řetězec</span><span class="sxs-lookup"><span data-stu-id="1e169-147">String</span></span>|<span data-ttu-id="1e169-148">Id uzlu, na kterém byla úloha spuštěna.</span><span class="sxs-lookup"><span data-stu-id="1e169-148">The id of the node on which the task ran.</span></span>|

###  <span data-ttu-id="1e169-149"><a name="multiInstanceSettings"></a>multiInstanceSettings</span><span class="sxs-lookup"><span data-stu-id="1e169-149"><a name="multiInstanceSettings"></a> multiInstanceSettings</span></span>

|<span data-ttu-id="1e169-150">Název elementu</span><span class="sxs-lookup"><span data-stu-id="1e169-150">Element name</span></span>|<span data-ttu-id="1e169-151">Typ</span><span class="sxs-lookup"><span data-stu-id="1e169-151">Type</span></span>|<span data-ttu-id="1e169-152">Poznámky</span><span class="sxs-lookup"><span data-stu-id="1e169-152">Notes</span></span>|
|------------------|----------|-----------|
|<span data-ttu-id="1e169-153">numberOfInstances</span><span class="sxs-lookup"><span data-stu-id="1e169-153">numberOfInstances</span></span>|<span data-ttu-id="1e169-154">Int32</span><span class="sxs-lookup"><span data-stu-id="1e169-154">Int32</span></span>|<span data-ttu-id="1e169-155">Celkový počet výpočetních uzlů požadovaných úkolem</span><span class="sxs-lookup"><span data-stu-id="1e169-155">The number of compute nodes required by the task.</span></span>|

###  <span data-ttu-id="1e169-156"><a name="constraints"></a>omezení</span><span class="sxs-lookup"><span data-stu-id="1e169-156"><a name="constraints"></a> constraints</span></span>

|<span data-ttu-id="1e169-157">Název elementu</span><span class="sxs-lookup"><span data-stu-id="1e169-157">Element name</span></span>|<span data-ttu-id="1e169-158">Typ</span><span class="sxs-lookup"><span data-stu-id="1e169-158">Type</span></span>|<span data-ttu-id="1e169-159">Poznámky</span><span class="sxs-lookup"><span data-stu-id="1e169-159">Notes</span></span>|
|------------------|----------|-----------|
|<span data-ttu-id="1e169-160">maxTaskRetryCount</span><span class="sxs-lookup"><span data-stu-id="1e169-160">maxTaskRetryCount</span></span>|<span data-ttu-id="1e169-161">Int32</span><span class="sxs-lookup"><span data-stu-id="1e169-161">Int32</span></span>|<span data-ttu-id="1e169-162">Maximální počet, který může být Opakovat úlohu.</span><span class="sxs-lookup"><span data-stu-id="1e169-162">The maximum number of times the task may be retried.</span></span> <span data-ttu-id="1e169-163">Služba Batch úkol zopakuje, pokud je jeho ukončovací kód nenulové hodnoty.</span><span class="sxs-lookup"><span data-stu-id="1e169-163">The Batch service retries a task if its exit code is nonzero.</span></span><br /><br /> <span data-ttu-id="1e169-164">Všimněte si, že tato hodnota konkrétně určuje počet opakovaných pokusů.</span><span class="sxs-lookup"><span data-stu-id="1e169-164">Note that this value specifically controls the number of retries.</span></span> <span data-ttu-id="1e169-165">Služba Batch bude opakujte úlohu jednou a mohou zkuste až toto omezení.</span><span class="sxs-lookup"><span data-stu-id="1e169-165">The Batch service will try the task once, and may then retry up to this limit.</span></span> <span data-ttu-id="1e169-166">Například pokud je maximální počet opakování je 3, Batch pokusů a úloh až 4 případech (jeden počáteční pokus a 3 opakování).</span><span class="sxs-lookup"><span data-stu-id="1e169-166">For example, if the maximum retry count is 3, Batch tries a task up to 4 times (one initial try and 3 retries).</span></span><br /><br /> <span data-ttu-id="1e169-167">Pokud je maximální počet opakování 0, služba Batch neopakuje úlohy.</span><span class="sxs-lookup"><span data-stu-id="1e169-167">If the maximum retry count is 0, the Batch service does not retry tasks.</span></span><br /><br /> <span data-ttu-id="1e169-168">Pokud je maximální počet opakování −1, služba Batch opakuje úlohy bez omezení.</span><span class="sxs-lookup"><span data-stu-id="1e169-168">If the maximum retry count is -1, the Batch service retries tasks without limit.</span></span><br /><br /> <span data-ttu-id="1e169-169">Výchozí hodnota je 0 (bez opakování).</span><span class="sxs-lookup"><span data-stu-id="1e169-169">The default value is 0 (no retries).</span></span>|

###  <span data-ttu-id="1e169-170"><a name="executionInfo"></a>executionInfo</span><span class="sxs-lookup"><span data-stu-id="1e169-170"><a name="executionInfo"></a> executionInfo</span></span>

|<span data-ttu-id="1e169-171">Název elementu</span><span class="sxs-lookup"><span data-stu-id="1e169-171">Element name</span></span>|<span data-ttu-id="1e169-172">Typ</span><span class="sxs-lookup"><span data-stu-id="1e169-172">Type</span></span>|<span data-ttu-id="1e169-173">Poznámky</span><span class="sxs-lookup"><span data-stu-id="1e169-173">Notes</span></span>|
|------------------|----------|-----------|
|<span data-ttu-id="1e169-174">startTime</span><span class="sxs-lookup"><span data-stu-id="1e169-174">startTime</span></span>|<span data-ttu-id="1e169-175">Data a času</span><span class="sxs-lookup"><span data-stu-id="1e169-175">DateTime</span></span>|<span data-ttu-id="1e169-176">Čas, kdy úloha spustí systémem.</span><span class="sxs-lookup"><span data-stu-id="1e169-176">The time at which the task started running.</span></span> <span data-ttu-id="1e169-177">"Spuštěný" odpovídá **systémem** stavu, takže pokud úloha Určuje soubory prostředků nebo balíčky aplikací, pak počáteční čas zobrazí dobu, kdy úloha spustí stahování nebo nasazování těchto.</span><span class="sxs-lookup"><span data-stu-id="1e169-177">'Running' corresponds to the **running** state, so if the task specifies resource files or application packages, then the start time reflects the time at which the task started downloading or deploying these.</span></span>  <span data-ttu-id="1e169-178">Pokud byl restartován nebo Opakovat úlohu, je to poslední čas, kdy úloha spustí systémem.</span><span class="sxs-lookup"><span data-stu-id="1e169-178">If the task has been restarted or retried, this is the most recent time at which the task started running.</span></span>|
|<span data-ttu-id="1e169-179">čas ukončení</span><span class="sxs-lookup"><span data-stu-id="1e169-179">endTime</span></span>|<span data-ttu-id="1e169-180">Data a času</span><span class="sxs-lookup"><span data-stu-id="1e169-180">DateTime</span></span>|<span data-ttu-id="1e169-181">Čas, kdy je úloha dokončena.</span><span class="sxs-lookup"><span data-stu-id="1e169-181">The time at which the task completed.</span></span>|
|<span data-ttu-id="1e169-182">exitCode</span><span class="sxs-lookup"><span data-stu-id="1e169-182">exitCode</span></span>|<span data-ttu-id="1e169-183">Int32</span><span class="sxs-lookup"><span data-stu-id="1e169-183">Int32</span></span>|<span data-ttu-id="1e169-184">Kód ukončení úlohy.</span><span class="sxs-lookup"><span data-stu-id="1e169-184">The exit code of the task.</span></span>|
|<span data-ttu-id="1e169-185">retryCount</span><span class="sxs-lookup"><span data-stu-id="1e169-185">retryCount</span></span>|<span data-ttu-id="1e169-186">Int32</span><span class="sxs-lookup"><span data-stu-id="1e169-186">Int32</span></span>|<span data-ttu-id="1e169-187">Počet, kolikrát má byla úloha opakovat službou Batch.</span><span class="sxs-lookup"><span data-stu-id="1e169-187">The number of times the task has been retried by the Batch service.</span></span> <span data-ttu-id="1e169-188">Úlohy se pokus o Pokud ukončí nenulový ukončovací kód, až do zadaného MaxTaskRetryCount.</span><span class="sxs-lookup"><span data-stu-id="1e169-188">The task is retried if it exits with a nonzero exit code, up to the specified MaxTaskRetryCount.</span></span>|
|<span data-ttu-id="1e169-189">requeueCount</span><span class="sxs-lookup"><span data-stu-id="1e169-189">requeueCount</span></span>|<span data-ttu-id="1e169-190">Int32</span><span class="sxs-lookup"><span data-stu-id="1e169-190">Int32</span></span>|<span data-ttu-id="1e169-191">Počet, kolikrát má byla zařazena úkol službou Batch jako výsledek požadavku uživatele.</span><span class="sxs-lookup"><span data-stu-id="1e169-191">The number of times the task has been requeued by the Batch service as the result of a user request.</span></span><br /><br /> <span data-ttu-id="1e169-192">Pokud uzly odebere uživatele z fondu (nebo změnou velikosti zmenšení fondu) nebo když je úloha zakázaná, může uživatel zadat, že spuštění úlohy v uzlech být zařazena pro provedení.</span><span class="sxs-lookup"><span data-stu-id="1e169-192">When the user removes nodes from a pool (by resizing or shrinking the pool) or when the job is being disabled, the user can specify that running tasks on the nodes be requeued for execution.</span></span> <span data-ttu-id="1e169-193">Tento počet sleduje počet opakování úlohy byla zařazena. z těchto důvodů.</span><span class="sxs-lookup"><span data-stu-id="1e169-193">This count tracks how many times the task has been requeued for these reasons.</span></span>|
