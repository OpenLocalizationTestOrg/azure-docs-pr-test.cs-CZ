---
title: "Počáteční událost velikosti fondu Azure Batch | Microsoft Docs"
description: "Referenční dokumentace pro událost počáteční změny velikosti fondu Batch."
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
ms.openlocfilehash: 826cd984d26b923ba38562e05a2e75c399be9121
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="pool-resize-start-event"></a><span data-ttu-id="e905c-103">Událost spuštění změny velikosti fondu</span><span class="sxs-lookup"><span data-stu-id="e905c-103">Pool resize start event</span></span>

 <span data-ttu-id="e905c-104">Tato událost je vygenerované při bylo zahájeno změny velikosti fondu.</span><span class="sxs-lookup"><span data-stu-id="e905c-104">This event is emitted when a pool resize has started.</span></span> <span data-ttu-id="e905c-105">Vzhledem k tomu, že změny velikosti fondu je asynchronní událostí, můžete očekávat, že událost complete změny velikosti fondu pro vypuštění po dokončení operace změny velikosti.</span><span class="sxs-lookup"><span data-stu-id="e905c-105">Since the pool resize is an asynchronous event, you can expect a pool resize complete event to be emitted once the resize operation completes.</span></span>

 <span data-ttu-id="e905c-106">Následující příklad ukazuje text počáteční událost změny velikosti fondu pro fondu změnu velikosti od 0 do 2 uzlů se ruční změny velikosti.</span><span class="sxs-lookup"><span data-stu-id="e905c-106">The following example shows the body of a pool resize start event for a pool resizing from 0 to 2 nodes with a manual resize.</span></span>

```
{
    "poolId": "myPool1",
    "nodeDeallocationOption": "invalid",
    "currentDedicated": 0,
    "targetDedicated": 2,
    "enableAutoScale": false,
    "isAutoPool": false
}
```

|<span data-ttu-id="e905c-107">Element</span><span class="sxs-lookup"><span data-stu-id="e905c-107">Element</span></span>|<span data-ttu-id="e905c-108">Typ</span><span class="sxs-lookup"><span data-stu-id="e905c-108">Type</span></span>|<span data-ttu-id="e905c-109">Poznámky</span><span class="sxs-lookup"><span data-stu-id="e905c-109">Notes</span></span>|
|-------------|----------|-----------|
|<span data-ttu-id="e905c-110">poolId</span><span class="sxs-lookup"><span data-stu-id="e905c-110">poolId</span></span>|<span data-ttu-id="e905c-111">Řetězec</span><span class="sxs-lookup"><span data-stu-id="e905c-111">String</span></span>|<span data-ttu-id="e905c-112">Id fondu.</span><span class="sxs-lookup"><span data-stu-id="e905c-112">The id of the pool.</span></span>|
|<span data-ttu-id="e905c-113">nodeDeallocationOption</span><span class="sxs-lookup"><span data-stu-id="e905c-113">nodeDeallocationOption</span></span>|<span data-ttu-id="e905c-114">Řetězec</span><span class="sxs-lookup"><span data-stu-id="e905c-114">String</span></span>|<span data-ttu-id="e905c-115">Určuje, kdy může odebrat uzly ve fondu, pokud zmenšování velikosti fondu.</span><span class="sxs-lookup"><span data-stu-id="e905c-115">Specifies when nodes may be removed from the pool, if the pool size is decreasing.</span></span><br /><br /> <span data-ttu-id="e905c-116">Možné hodnoty:</span><span class="sxs-lookup"><span data-stu-id="e905c-116">Possible values are:</span></span><br /><br /> <span data-ttu-id="e905c-117">**requeue** – ukončí se všechny spuštěné úlohy a znovu je.</span><span class="sxs-lookup"><span data-stu-id="e905c-117">**requeue** – Terminate running tasks and requeue them.</span></span> <span data-ttu-id="e905c-118">Úkoly se znovu spustí, když bude povolena úloha.</span><span class="sxs-lookup"><span data-stu-id="e905c-118">The tasks will run again when the job is enabled.</span></span> <span data-ttu-id="e905c-119">Odeberte uzly, jakmile úkoly ukončí.</span><span class="sxs-lookup"><span data-stu-id="e905c-119">Remove nodes as soon as tasks have been terminated.</span></span><br /><br /> <span data-ttu-id="e905c-120">**Ukončit** – ukončit spuštěné úkoly.</span><span class="sxs-lookup"><span data-stu-id="e905c-120">**terminate** – Terminate running tasks.</span></span> <span data-ttu-id="e905c-121">Úkoly se znovu nespustí.</span><span class="sxs-lookup"><span data-stu-id="e905c-121">The tasks will not run again.</span></span> <span data-ttu-id="e905c-122">Odeberte uzly, jakmile úkoly ukončí.</span><span class="sxs-lookup"><span data-stu-id="e905c-122">Remove nodes as soon as tasks have been terminated.</span></span><br /><br /> <span data-ttu-id="e905c-123">**taskcompletion** – povolit aktuálně spuštěných úloh k dokončení.</span><span class="sxs-lookup"><span data-stu-id="e905c-123">**taskcompletion** – Allow currently running tasks to complete.</span></span> <span data-ttu-id="e905c-124">Plánovat žádné nové úkoly při čekání.</span><span class="sxs-lookup"><span data-stu-id="e905c-124">Schedule no new tasks while waiting.</span></span> <span data-ttu-id="e905c-125">Odeberte uzly po dokončení všech úloh.</span><span class="sxs-lookup"><span data-stu-id="e905c-125">Remove nodes when all tasks have completed.</span></span><br /><br /> <span data-ttu-id="e905c-126">**Retaineddata** -povolit aktuálně spuštěných úkolů dokončily. pak počkejte, než všech úkolů období uchovávání dat vyprší.</span><span class="sxs-lookup"><span data-stu-id="e905c-126">**Retaineddata** - Allow currently running tasks to complete, then wait for all task data retention periods to expire.</span></span> <span data-ttu-id="e905c-127">Plánovat žádné nové úkoly při čekání.</span><span class="sxs-lookup"><span data-stu-id="e905c-127">Schedule no new tasks while waiting.</span></span> <span data-ttu-id="e905c-128">Odeberte uzly, pokud vypršela platnost období uchovávání všech úloh.</span><span class="sxs-lookup"><span data-stu-id="e905c-128">Remove nodes when all task retention periods have expired.</span></span><br /><br /> <span data-ttu-id="e905c-129">Výchozí hodnota je requeue.</span><span class="sxs-lookup"><span data-stu-id="e905c-129">The default value is requeue.</span></span><br /><br /> <span data-ttu-id="e905c-130">Pokud je zvýšení velikosti fondu, pak je hodnota nastavená **neplatný**.</span><span class="sxs-lookup"><span data-stu-id="e905c-130">If the pool size is increasing then the value is set to **invalid**.</span></span>|
|<span data-ttu-id="e905c-131">currentDedicated</span><span class="sxs-lookup"><span data-stu-id="e905c-131">currentDedicated</span></span>|<span data-ttu-id="e905c-132">Int32</span><span class="sxs-lookup"><span data-stu-id="e905c-132">Int32</span></span>|<span data-ttu-id="e905c-133">Počet aktuálně přiřazená k fondu výpočetních uzlů.</span><span class="sxs-lookup"><span data-stu-id="e905c-133">The number of compute nodes currently assigned to the pool.</span></span>|
|<span data-ttu-id="e905c-134">targetDedicated</span><span class="sxs-lookup"><span data-stu-id="e905c-134">targetDedicated</span></span>|<span data-ttu-id="e905c-135">Int32</span><span class="sxs-lookup"><span data-stu-id="e905c-135">Int32</span></span>|<span data-ttu-id="e905c-136">Počet výpočetních uzlů, které jsou požadovány pro fond.</span><span class="sxs-lookup"><span data-stu-id="e905c-136">The number of compute nodes that are requested for the pool.</span></span>|
|<span data-ttu-id="e905c-137">enableAutoScale</span><span class="sxs-lookup"><span data-stu-id="e905c-137">enableAutoScale</span></span>|<span data-ttu-id="e905c-138">BOOL</span><span class="sxs-lookup"><span data-stu-id="e905c-138">Bool</span></span>|<span data-ttu-id="e905c-139">Určuje, zda velikost fondu automaticky přizpůsobí v čase.</span><span class="sxs-lookup"><span data-stu-id="e905c-139">Specifies whether the pool size automatically adjusts over time.</span></span>|
|<span data-ttu-id="e905c-140">isAutoPool</span><span class="sxs-lookup"><span data-stu-id="e905c-140">isAutoPool</span></span>|<span data-ttu-id="e905c-141">BOOL</span><span class="sxs-lookup"><span data-stu-id="e905c-141">Bool</span></span>|<span data-ttu-id="e905c-142">Speficies jestli fondu se vytvořil prostřednictvím mechanismu AutoPool úlohy.</span><span class="sxs-lookup"><span data-stu-id="e905c-142">Speficies whether the pool was created via a job's AutoPool mechanism.</span></span>|
