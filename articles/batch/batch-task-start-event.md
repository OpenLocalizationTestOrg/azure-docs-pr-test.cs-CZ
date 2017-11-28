---
title: "AAA \"události spuštění úlohy Azure Batch | Microsoft Docs\""
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
ms.openlocfilehash: 2cb066be1578741125e9081a84a2b7c74dc8356a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="task-start-event"></a><span data-ttu-id="bb319-103">Událost spuštění úlohy</span><span class="sxs-lookup"><span data-stu-id="bb319-103">Task start event</span></span>

 <span data-ttu-id="bb319-104">Tato událost je vygenerované Jakmile úloha už naplánované toostart na výpočetním uzlu plánovačem hello.</span><span class="sxs-lookup"><span data-stu-id="bb319-104">This event is emitted once a task has been scheduled toostart on a compute node by hello scheduler.</span></span> <span data-ttu-id="bb319-105">Všimněte si, že pokud je úloha hello opakovat nebo zařazena Tato událost bude být vygenerované znovu hello stejný úkol, ale hello počet opakování a verze systému úloh bude příslušným způsobem aktualizuje.</span><span class="sxs-lookup"><span data-stu-id="bb319-105">Note that if hello task is retried or requeued this event will be emitted again for hello same task, but hello retry count and system task version will be updated accordingly.</span></span>


 <span data-ttu-id="bb319-106">Hello následující příklad ukazuje textu hello události spuštění úloh.</span><span class="sxs-lookup"><span data-stu-id="bb319-106">hello following example shows hello body of a task start event.</span></span>

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

|<span data-ttu-id="bb319-107">Název elementu</span><span class="sxs-lookup"><span data-stu-id="bb319-107">Element name</span></span>|<span data-ttu-id="bb319-108">Typ</span><span class="sxs-lookup"><span data-stu-id="bb319-108">Type</span></span>|<span data-ttu-id="bb319-109">Poznámky</span><span class="sxs-lookup"><span data-stu-id="bb319-109">Notes</span></span>|
|------------------|----------|-----------|
|<span data-ttu-id="bb319-110">JobId</span><span class="sxs-lookup"><span data-stu-id="bb319-110">jobId</span></span>|<span data-ttu-id="bb319-111">Řetězec</span><span class="sxs-lookup"><span data-stu-id="bb319-111">String</span></span>|<span data-ttu-id="bb319-112">id Hello hello úlohy, která obsahuje úlohu hello.</span><span class="sxs-lookup"><span data-stu-id="bb319-112">hello id of hello job containing hello task.</span></span>|
|<span data-ttu-id="bb319-113">id</span><span class="sxs-lookup"><span data-stu-id="bb319-113">id</span></span>|<span data-ttu-id="bb319-114">Řetězec</span><span class="sxs-lookup"><span data-stu-id="bb319-114">String</span></span>|<span data-ttu-id="bb319-115">id Hello hello úlohy.</span><span class="sxs-lookup"><span data-stu-id="bb319-115">hello id of hello task.</span></span>|
|<span data-ttu-id="bb319-116">taskType</span><span class="sxs-lookup"><span data-stu-id="bb319-116">taskType</span></span>|<span data-ttu-id="bb319-117">Řetězec</span><span class="sxs-lookup"><span data-stu-id="bb319-117">String</span></span>|<span data-ttu-id="bb319-118">Typ Hello hello úlohy.</span><span class="sxs-lookup"><span data-stu-id="bb319-118">hello type of hello task.</span></span> <span data-ttu-id="bb319-119">To může být JobManager oznamující, že je úkol správce nebo uživatel oznamující, že se nejedná o úkolu Správce úloh.</span><span class="sxs-lookup"><span data-stu-id="bb319-119">This can either be 'JobManager' indicating it is a job manager task or 'User' indicating it is not a job manager task.</span></span>|
|<span data-ttu-id="bb319-120">systemTaskVersion</span><span class="sxs-lookup"><span data-stu-id="bb319-120">systemTaskVersion</span></span>|<span data-ttu-id="bb319-121">Int32</span><span class="sxs-lookup"><span data-stu-id="bb319-121">Int32</span></span>|<span data-ttu-id="bb319-122">Toto je interní opakování čítač hello na úlohu.</span><span class="sxs-lookup"><span data-stu-id="bb319-122">This is hello internal retry counter on a task.</span></span> <span data-ttu-id="bb319-123">Služba Batch hello interně může pokus zopakovat tooaccount úloh pro přechodné problémy.</span><span class="sxs-lookup"><span data-stu-id="bb319-123">Internally hello Batch service can retry a task tooaccount for transient issues.</span></span> <span data-ttu-id="bb319-124">Tyto problémy mohou zahrnovat interní plánování chyby nebo pokusy o toorecover z výpočetních uzlů ve špatném stavu.</span><span class="sxs-lookup"><span data-stu-id="bb319-124">These issues can include internal scheduling errors or attempts toorecover from compute nodes in a bad state.</span></span>|
|[<span data-ttu-id="bb319-125">nodeInfo</span><span class="sxs-lookup"><span data-stu-id="bb319-125">nodeInfo</span></span>](#nodeInfo)|<span data-ttu-id="bb319-126">Komplexní typ</span><span class="sxs-lookup"><span data-stu-id="bb319-126">Complex Type</span></span>|<span data-ttu-id="bb319-127">Obsahuje informace o hello výpočetním uzlu, na které hello úloha spustila.</span><span class="sxs-lookup"><span data-stu-id="bb319-127">Contains information about hello compute node on which hello task ran.</span></span>|
|[<span data-ttu-id="bb319-128">multiInstanceSettings</span><span class="sxs-lookup"><span data-stu-id="bb319-128">multiInstanceSettings</span></span>](#multiInstanceSettings)|<span data-ttu-id="bb319-129">Komplexní typ</span><span class="sxs-lookup"><span data-stu-id="bb319-129">Complex Type</span></span>|<span data-ttu-id="bb319-130">Určuje, že tuto úlohu hello je úkol s více instancemi nutnosti několika výpočetních uzlech.</span><span class="sxs-lookup"><span data-stu-id="bb319-130">Specifies that hello task  is Multi-Instance Task requiring multiple compute nodes.</span></span>  <span data-ttu-id="bb319-131">V tématu [multiInstanceSettings](https://docs.microsoft.com/rest/api/batchservice/get-information-about-a-task) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="bb319-131">See [multiInstanceSettings](https://docs.microsoft.com/rest/api/batchservice/get-information-about-a-task) for details.</span></span>|
|[<span data-ttu-id="bb319-132">omezení</span><span class="sxs-lookup"><span data-stu-id="bb319-132">constraints</span></span>](#constraints)|<span data-ttu-id="bb319-133">Komplexní typ</span><span class="sxs-lookup"><span data-stu-id="bb319-133">Complex Type</span></span>|<span data-ttu-id="bb319-134">Hello provádění omezení, která platí toothis úloh.</span><span class="sxs-lookup"><span data-stu-id="bb319-134">hello execution constraints that apply toothis task.</span></span>|
|[<span data-ttu-id="bb319-135">executionInfo</span><span class="sxs-lookup"><span data-stu-id="bb319-135">executionInfo</span></span>](#executionInfo)|<span data-ttu-id="bb319-136">Komplexní typ</span><span class="sxs-lookup"><span data-stu-id="bb319-136">Complex Type</span></span>|<span data-ttu-id="bb319-137">Obsahuje informace o provádění hello hello úlohy.</span><span class="sxs-lookup"><span data-stu-id="bb319-137">Contains information about hello execution of hello task.</span></span>|

###  <span data-ttu-id="bb319-138"><a name="nodeInfo"></a>nodeInfo</span><span class="sxs-lookup"><span data-stu-id="bb319-138"><a name="nodeInfo"></a> nodeInfo</span></span>

|<span data-ttu-id="bb319-139">Název elementu</span><span class="sxs-lookup"><span data-stu-id="bb319-139">Element name</span></span>|<span data-ttu-id="bb319-140">Typ</span><span class="sxs-lookup"><span data-stu-id="bb319-140">Type</span></span>|<span data-ttu-id="bb319-141">Poznámky</span><span class="sxs-lookup"><span data-stu-id="bb319-141">Notes</span></span>|
|------------------|----------|-----------|
|<span data-ttu-id="bb319-142">poolId</span><span class="sxs-lookup"><span data-stu-id="bb319-142">poolId</span></span>|<span data-ttu-id="bb319-143">Řetězec</span><span class="sxs-lookup"><span data-stu-id="bb319-143">String</span></span>|<span data-ttu-id="bb319-144">id Hello hello fond, na které hello úloha spustila.</span><span class="sxs-lookup"><span data-stu-id="bb319-144">hello id of hello pool on which hello task ran.</span></span>|
|<span data-ttu-id="bb319-145">nodeId</span><span class="sxs-lookup"><span data-stu-id="bb319-145">nodeId</span></span>|<span data-ttu-id="bb319-146">Řetězec</span><span class="sxs-lookup"><span data-stu-id="bb319-146">String</span></span>|<span data-ttu-id="bb319-147">id Hello hello uzlu, na které hello úloha spustila.</span><span class="sxs-lookup"><span data-stu-id="bb319-147">hello id of hello node on which hello task ran.</span></span>|

###  <span data-ttu-id="bb319-148"><a name="multiInstanceSettings"></a>multiInstanceSettings</span><span class="sxs-lookup"><span data-stu-id="bb319-148"><a name="multiInstanceSettings"></a> multiInstanceSettings</span></span>

|<span data-ttu-id="bb319-149">Název elementu</span><span class="sxs-lookup"><span data-stu-id="bb319-149">Element name</span></span>|<span data-ttu-id="bb319-150">Typ</span><span class="sxs-lookup"><span data-stu-id="bb319-150">Type</span></span>|<span data-ttu-id="bb319-151">Poznámky</span><span class="sxs-lookup"><span data-stu-id="bb319-151">Notes</span></span>|
|------------------|----------|-----------|
|<span data-ttu-id="bb319-152">numberOfInstances</span><span class="sxs-lookup"><span data-stu-id="bb319-152">numberOfInstances</span></span>|<span data-ttu-id="bb319-153">celá čísla</span><span class="sxs-lookup"><span data-stu-id="bb319-153">Int</span></span>|<span data-ttu-id="bb319-154">Hello počet výpočetních uzlů, které vyžadují hello úloh.</span><span class="sxs-lookup"><span data-stu-id="bb319-154">hello number of compute nodes required by hello task.</span></span>|

###  <span data-ttu-id="bb319-155"><a name="constraints"></a>omezení</span><span class="sxs-lookup"><span data-stu-id="bb319-155"><a name="constraints"></a> constraints</span></span>

|<span data-ttu-id="bb319-156">Název elementu</span><span class="sxs-lookup"><span data-stu-id="bb319-156">Element name</span></span>|<span data-ttu-id="bb319-157">Typ</span><span class="sxs-lookup"><span data-stu-id="bb319-157">Type</span></span>|<span data-ttu-id="bb319-158">Poznámky</span><span class="sxs-lookup"><span data-stu-id="bb319-158">Notes</span></span>|
|------------------|----------|-----------|
|<span data-ttu-id="bb319-159">maxTaskRetryCount</span><span class="sxs-lookup"><span data-stu-id="bb319-159">maxTaskRetryCount</span></span>|<span data-ttu-id="bb319-160">Int32</span><span class="sxs-lookup"><span data-stu-id="bb319-160">Int32</span></span>|<span data-ttu-id="bb319-161">Hello maximální počet opakovaných úkolů hello.</span><span class="sxs-lookup"><span data-stu-id="bb319-161">hello maximum number of times hello task may be retried.</span></span> <span data-ttu-id="bb319-162">Hello služba Batch úkol zopakuje, pokud je jeho ukončovací kód nenulové hodnoty.</span><span class="sxs-lookup"><span data-stu-id="bb319-162">hello Batch service retries a task if its exit code is nonzero.</span></span><br /><br /> <span data-ttu-id="bb319-163">Všimněte si, že tato hodnota řídí konkrétně hello počet opakování.</span><span class="sxs-lookup"><span data-stu-id="bb319-163">Note that this value specifically controls hello number of retries.</span></span> <span data-ttu-id="bb319-164">Služba Batch Hello se pokusí hello úloh jednou a mohou zkuste si toothis limit.</span><span class="sxs-lookup"><span data-stu-id="bb319-164">hello Batch service will try hello task once, and may then retry up toothis limit.</span></span> <span data-ttu-id="bb319-165">Například pokud hello maximální počet opakování je 3, Batch pokusí úlohu až too4 dobu (jeden počáteční pokus a 3 opakování).</span><span class="sxs-lookup"><span data-stu-id="bb319-165">For example, if hello maximum retry count is 3, Batch tries a task up too4 times (one initial try and 3 retries).</span></span><br /><br /> <span data-ttu-id="bb319-166">Pokud hello maximální počet opakování 0, služba Batch hello neopakuje úlohy.</span><span class="sxs-lookup"><span data-stu-id="bb319-166">If hello maximum retry count is 0, hello Batch service does not retry tasks.</span></span><br /><br /> <span data-ttu-id="bb319-167">Pokud hello maximální počet opakování −1, služba Batch hello opakuje úlohy bez omezení.</span><span class="sxs-lookup"><span data-stu-id="bb319-167">If hello maximum retry count is -1, hello Batch service retries tasks without limit.</span></span><br /><br /> <span data-ttu-id="bb319-168">Hello výchozí hodnota je 0 (bez opakování).</span><span class="sxs-lookup"><span data-stu-id="bb319-168">hello default value is 0 (no retries).</span></span>|

###  <span data-ttu-id="bb319-169"><a name="executionInfo"></a>executionInfo</span><span class="sxs-lookup"><span data-stu-id="bb319-169"><a name="executionInfo"></a> executionInfo</span></span>

|<span data-ttu-id="bb319-170">Název elementu</span><span class="sxs-lookup"><span data-stu-id="bb319-170">Element name</span></span>|<span data-ttu-id="bb319-171">Typ</span><span class="sxs-lookup"><span data-stu-id="bb319-171">Type</span></span>|<span data-ttu-id="bb319-172">Poznámky</span><span class="sxs-lookup"><span data-stu-id="bb319-172">Notes</span></span>|
|------------------|----------|-----------|
|<span data-ttu-id="bb319-173">retryCount</span><span class="sxs-lookup"><span data-stu-id="bb319-173">retryCount</span></span>|<span data-ttu-id="bb319-174">Int32</span><span class="sxs-lookup"><span data-stu-id="bb319-174">Int32</span></span>|<span data-ttu-id="bb319-175">Hello počet oznámení, která má byla hello úloha opakovat hello služby Batch.</span><span class="sxs-lookup"><span data-stu-id="bb319-175">hello number of times hello task has been retried by hello Batch service.</span></span> <span data-ttu-id="bb319-176">Úloha Hello je zopakován, pokud bude nenulový ukončovací kód, až toohello zadaný MaxTaskRetryCount</span><span class="sxs-lookup"><span data-stu-id="bb319-176">hello task is retried if it exits with a nonzero exit code, up toohello specified MaxTaskRetryCount</span></span>|
