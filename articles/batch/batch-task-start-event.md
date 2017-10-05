---
title: "Azure události spuštění úlohy Batch | Microsoft Docs"
description: "Referenční informace pro úlohy Batch spusťte událost."
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
ms.openlocfilehash: c47ab36c99dddd46a14c15018a2a46bf7f873ffa
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="task-start-event"></a><span data-ttu-id="4958a-103">Událost spuštění úlohy</span><span class="sxs-lookup"><span data-stu-id="4958a-103">Task start event</span></span>

 <span data-ttu-id="4958a-104">Tato událost je vygenerované po úlohu bylo naplánováno spuštění na výpočetním uzlu plánovačem.</span><span class="sxs-lookup"><span data-stu-id="4958a-104">This event is emitted once a task has been scheduled to start on a compute node by the scheduler.</span></span> <span data-ttu-id="4958a-105">Upozorňujeme, že pokud je úloha opakovat nebo zařazena Tato událost bude znovu vygenerované pro stejnou úlohu, ale počet opakování, a verze systému úloh bude příslušným způsobem aktualizuje.</span><span class="sxs-lookup"><span data-stu-id="4958a-105">Note that if the task is retried or requeued this event will be emitted again for the same task, but the retry count and system task version will be updated accordingly.</span></span>


 <span data-ttu-id="4958a-106">Následující příklad ukazuje, text úlohu spustit událostí.</span><span class="sxs-lookup"><span data-stu-id="4958a-106">The following example shows the body of a task start event.</span></span>

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
        "retryCount": 0
    }
}
```

|<span data-ttu-id="4958a-107">Název elementu</span><span class="sxs-lookup"><span data-stu-id="4958a-107">Element name</span></span>|<span data-ttu-id="4958a-108">Typ</span><span class="sxs-lookup"><span data-stu-id="4958a-108">Type</span></span>|<span data-ttu-id="4958a-109">Poznámky</span><span class="sxs-lookup"><span data-stu-id="4958a-109">Notes</span></span>|
|------------------|----------|-----------|
|<span data-ttu-id="4958a-110">JobId</span><span class="sxs-lookup"><span data-stu-id="4958a-110">jobId</span></span>|<span data-ttu-id="4958a-111">Řetězec</span><span class="sxs-lookup"><span data-stu-id="4958a-111">String</span></span>|<span data-ttu-id="4958a-112">Id úlohy, která obsahuje úlohu.</span><span class="sxs-lookup"><span data-stu-id="4958a-112">The id of the job containing the task.</span></span>|
|<span data-ttu-id="4958a-113">id</span><span class="sxs-lookup"><span data-stu-id="4958a-113">id</span></span>|<span data-ttu-id="4958a-114">Řetězec</span><span class="sxs-lookup"><span data-stu-id="4958a-114">String</span></span>|<span data-ttu-id="4958a-115">Id úkolu.</span><span class="sxs-lookup"><span data-stu-id="4958a-115">The id of the task.</span></span>|
|<span data-ttu-id="4958a-116">taskType</span><span class="sxs-lookup"><span data-stu-id="4958a-116">taskType</span></span>|<span data-ttu-id="4958a-117">Řetězec</span><span class="sxs-lookup"><span data-stu-id="4958a-117">String</span></span>|<span data-ttu-id="4958a-118">Typ úlohy.</span><span class="sxs-lookup"><span data-stu-id="4958a-118">The type of the task.</span></span> <span data-ttu-id="4958a-119">To může být JobManager oznamující, že je úkol správce nebo uživatel oznamující, že se nejedná o úkolu Správce úloh.</span><span class="sxs-lookup"><span data-stu-id="4958a-119">This can either be 'JobManager' indicating it is a job manager task or 'User' indicating it is not a job manager task.</span></span>|
|<span data-ttu-id="4958a-120">systemTaskVersion</span><span class="sxs-lookup"><span data-stu-id="4958a-120">systemTaskVersion</span></span>|<span data-ttu-id="4958a-121">Int32</span><span class="sxs-lookup"><span data-stu-id="4958a-121">Int32</span></span>|<span data-ttu-id="4958a-122">Toto je Čítač opakovaných pokusů interní na úlohu.</span><span class="sxs-lookup"><span data-stu-id="4958a-122">This is the internal retry counter on a task.</span></span> <span data-ttu-id="4958a-123">Služba Batch interně může pokus zopakovat úlohu, aby se zohlednily přechodné problémy.</span><span class="sxs-lookup"><span data-stu-id="4958a-123">Internally the Batch service can retry a task to account for transient issues.</span></span> <span data-ttu-id="4958a-124">Tyto problémy mohou zahrnovat plánování s interními chybami nebo pokusy o obnovení z výpočetních uzlů ve špatném stavu.</span><span class="sxs-lookup"><span data-stu-id="4958a-124">These issues can include internal scheduling errors or attempts to recover from compute nodes in a bad state.</span></span>|
|[<span data-ttu-id="4958a-125">nodeInfo</span><span class="sxs-lookup"><span data-stu-id="4958a-125">nodeInfo</span></span>](#nodeInfo)|<span data-ttu-id="4958a-126">Komplexní typ</span><span class="sxs-lookup"><span data-stu-id="4958a-126">Complex Type</span></span>|<span data-ttu-id="4958a-127">Obsahuje informace o výpočetním uzlu, na kterém byla úloha spuštěna.</span><span class="sxs-lookup"><span data-stu-id="4958a-127">Contains information about the compute node on which the task ran.</span></span>|
|[<span data-ttu-id="4958a-128">multiInstanceSettings</span><span class="sxs-lookup"><span data-stu-id="4958a-128">multiInstanceSettings</span></span>](#multiInstanceSettings)|<span data-ttu-id="4958a-129">Komplexní typ</span><span class="sxs-lookup"><span data-stu-id="4958a-129">Complex Type</span></span>|<span data-ttu-id="4958a-130">Určuje, že tato úloha je úkol s více instancemi nutnosti několika výpočetních uzlech.</span><span class="sxs-lookup"><span data-stu-id="4958a-130">Specifies that the task  is Multi-Instance Task requiring multiple compute nodes.</span></span>  <span data-ttu-id="4958a-131">V tématu [multiInstanceSettings](https://docs.microsoft.com/rest/api/batchservice/get-information-about-a-task) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="4958a-131">See [multiInstanceSettings](https://docs.microsoft.com/rest/api/batchservice/get-information-about-a-task) for details.</span></span>|
|[<span data-ttu-id="4958a-132">omezení</span><span class="sxs-lookup"><span data-stu-id="4958a-132">constraints</span></span>](#constraints)|<span data-ttu-id="4958a-133">Komplexní typ</span><span class="sxs-lookup"><span data-stu-id="4958a-133">Complex Type</span></span>|<span data-ttu-id="4958a-134">Provádění omezení, které se vztahují k tomuto úkolu.</span><span class="sxs-lookup"><span data-stu-id="4958a-134">The execution constraints that apply to this task.</span></span>|
|[<span data-ttu-id="4958a-135">executionInfo</span><span class="sxs-lookup"><span data-stu-id="4958a-135">executionInfo</span></span>](#executionInfo)|<span data-ttu-id="4958a-136">Komplexní typ</span><span class="sxs-lookup"><span data-stu-id="4958a-136">Complex Type</span></span>|<span data-ttu-id="4958a-137">Obsahuje informace o provádění úlohy.</span><span class="sxs-lookup"><span data-stu-id="4958a-137">Contains information about the execution of the task.</span></span>|

###  <span data-ttu-id="4958a-138"><a name="nodeInfo"></a>nodeInfo</span><span class="sxs-lookup"><span data-stu-id="4958a-138"><a name="nodeInfo"></a> nodeInfo</span></span>

|<span data-ttu-id="4958a-139">Název elementu</span><span class="sxs-lookup"><span data-stu-id="4958a-139">Element name</span></span>|<span data-ttu-id="4958a-140">Typ</span><span class="sxs-lookup"><span data-stu-id="4958a-140">Type</span></span>|<span data-ttu-id="4958a-141">Poznámky</span><span class="sxs-lookup"><span data-stu-id="4958a-141">Notes</span></span>|
|------------------|----------|-----------|
|<span data-ttu-id="4958a-142">poolId</span><span class="sxs-lookup"><span data-stu-id="4958a-142">poolId</span></span>|<span data-ttu-id="4958a-143">Řetězec</span><span class="sxs-lookup"><span data-stu-id="4958a-143">String</span></span>|<span data-ttu-id="4958a-144">Id fondu, ve kterém byla spuštěna úloha.</span><span class="sxs-lookup"><span data-stu-id="4958a-144">The id of the pool on which the task ran.</span></span>|
|<span data-ttu-id="4958a-145">nodeId</span><span class="sxs-lookup"><span data-stu-id="4958a-145">nodeId</span></span>|<span data-ttu-id="4958a-146">Řetězec</span><span class="sxs-lookup"><span data-stu-id="4958a-146">String</span></span>|<span data-ttu-id="4958a-147">Id uzlu, na kterém byla úloha spuštěna.</span><span class="sxs-lookup"><span data-stu-id="4958a-147">The id of the node on which the task ran.</span></span>|

###  <span data-ttu-id="4958a-148"><a name="multiInstanceSettings"></a>multiInstanceSettings</span><span class="sxs-lookup"><span data-stu-id="4958a-148"><a name="multiInstanceSettings"></a> multiInstanceSettings</span></span>

|<span data-ttu-id="4958a-149">Název elementu</span><span class="sxs-lookup"><span data-stu-id="4958a-149">Element name</span></span>|<span data-ttu-id="4958a-150">Typ</span><span class="sxs-lookup"><span data-stu-id="4958a-150">Type</span></span>|<span data-ttu-id="4958a-151">Poznámky</span><span class="sxs-lookup"><span data-stu-id="4958a-151">Notes</span></span>|
|------------------|----------|-----------|
|<span data-ttu-id="4958a-152">numberOfInstances</span><span class="sxs-lookup"><span data-stu-id="4958a-152">numberOfInstances</span></span>|<span data-ttu-id="4958a-153">celá čísla</span><span class="sxs-lookup"><span data-stu-id="4958a-153">Int</span></span>|<span data-ttu-id="4958a-154">Celkový počet výpočetních uzlů požadovaných úkolem</span><span class="sxs-lookup"><span data-stu-id="4958a-154">The number of compute nodes required by the task.</span></span>|

###  <span data-ttu-id="4958a-155"><a name="constraints"></a>omezení</span><span class="sxs-lookup"><span data-stu-id="4958a-155"><a name="constraints"></a> constraints</span></span>

|<span data-ttu-id="4958a-156">Název elementu</span><span class="sxs-lookup"><span data-stu-id="4958a-156">Element name</span></span>|<span data-ttu-id="4958a-157">Typ</span><span class="sxs-lookup"><span data-stu-id="4958a-157">Type</span></span>|<span data-ttu-id="4958a-158">Poznámky</span><span class="sxs-lookup"><span data-stu-id="4958a-158">Notes</span></span>|
|------------------|----------|-----------|
|<span data-ttu-id="4958a-159">maxTaskRetryCount</span><span class="sxs-lookup"><span data-stu-id="4958a-159">maxTaskRetryCount</span></span>|<span data-ttu-id="4958a-160">Int32</span><span class="sxs-lookup"><span data-stu-id="4958a-160">Int32</span></span>|<span data-ttu-id="4958a-161">Maximální počet, který může být Opakovat úlohu.</span><span class="sxs-lookup"><span data-stu-id="4958a-161">The maximum number of times the task may be retried.</span></span> <span data-ttu-id="4958a-162">Služba Batch úkol zopakuje, pokud je jeho ukončovací kód nenulové hodnoty.</span><span class="sxs-lookup"><span data-stu-id="4958a-162">The Batch service retries a task if its exit code is nonzero.</span></span><br /><br /> <span data-ttu-id="4958a-163">Všimněte si, že tato hodnota konkrétně určuje počet opakovaných pokusů.</span><span class="sxs-lookup"><span data-stu-id="4958a-163">Note that this value specifically controls the number of retries.</span></span> <span data-ttu-id="4958a-164">Služba Batch bude opakujte úlohu jednou a mohou zkuste až toto omezení.</span><span class="sxs-lookup"><span data-stu-id="4958a-164">The Batch service will try the task once, and may then retry up to this limit.</span></span> <span data-ttu-id="4958a-165">Například pokud je maximální počet opakování je 3, Batch pokusů a úloh až 4 případech (jeden počáteční pokus a 3 opakování).</span><span class="sxs-lookup"><span data-stu-id="4958a-165">For example, if the maximum retry count is 3, Batch tries a task up to 4 times (one initial try and 3 retries).</span></span><br /><br /> <span data-ttu-id="4958a-166">Pokud je maximální počet opakování 0, služba Batch neopakuje úlohy.</span><span class="sxs-lookup"><span data-stu-id="4958a-166">If the maximum retry count is 0, the Batch service does not retry tasks.</span></span><br /><br /> <span data-ttu-id="4958a-167">Pokud je maximální počet opakování −1, služba Batch opakuje úlohy bez omezení.</span><span class="sxs-lookup"><span data-stu-id="4958a-167">If the maximum retry count is -1, the Batch service retries tasks without limit.</span></span><br /><br /> <span data-ttu-id="4958a-168">Výchozí hodnota je 0 (bez opakování).</span><span class="sxs-lookup"><span data-stu-id="4958a-168">The default value is 0 (no retries).</span></span>|

###  <span data-ttu-id="4958a-169"><a name="executionInfo"></a>executionInfo</span><span class="sxs-lookup"><span data-stu-id="4958a-169"><a name="executionInfo"></a> executionInfo</span></span>

|<span data-ttu-id="4958a-170">Název elementu</span><span class="sxs-lookup"><span data-stu-id="4958a-170">Element name</span></span>|<span data-ttu-id="4958a-171">Typ</span><span class="sxs-lookup"><span data-stu-id="4958a-171">Type</span></span>|<span data-ttu-id="4958a-172">Poznámky</span><span class="sxs-lookup"><span data-stu-id="4958a-172">Notes</span></span>|
|------------------|----------|-----------|
|<span data-ttu-id="4958a-173">retryCount</span><span class="sxs-lookup"><span data-stu-id="4958a-173">retryCount</span></span>|<span data-ttu-id="4958a-174">Int32</span><span class="sxs-lookup"><span data-stu-id="4958a-174">Int32</span></span>|<span data-ttu-id="4958a-175">Počet, kolikrát má byla úloha opakovat službou Batch.</span><span class="sxs-lookup"><span data-stu-id="4958a-175">The number of times the task has been retried by the Batch service.</span></span> <span data-ttu-id="4958a-176">Úloha je opakovat, pokud ho ukončí nenulový ukončovací kód, až do zadaného MaxTaskRetryCount</span><span class="sxs-lookup"><span data-stu-id="4958a-176">The task is retried if it exits with a nonzero exit code, up to the specified MaxTaskRetryCount</span></span>|
