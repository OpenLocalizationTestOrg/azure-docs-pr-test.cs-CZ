---
title: "Událost complete resize fondu Azure Batch | Microsoft Docs"
description: "Referenční dokumentace pro fondu Batch změnit velikost událost complete."
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
ms.openlocfilehash: 7072293d98526812cb42ce9c2f8e33bfcafaa149
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="pool-resize-complete-event"></a><span data-ttu-id="cf3eb-103">Dokončení událost změny velikosti fondu</span><span class="sxs-lookup"><span data-stu-id="cf3eb-103">Pool resize complete event</span></span>

 <span data-ttu-id="cf3eb-104">Tato událost je vygenerované při změny velikosti fondu má byla dokončena nebo se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="cf3eb-104">This event is emitted when a pool resize has completed or failed.</span></span>

 <span data-ttu-id="cf3eb-105">Následující příklad ukazuje text událost complete změny velikosti fondu pro skupinu, která tento nárůst velikosti a byla úspěšně dokončena.</span><span class="sxs-lookup"><span data-stu-id="cf3eb-105">The following example shows the body of a pool resize complete event for a pool that increased in size and completed successfully.</span></span>

```
{
    "id": "p_1_0_01503750-252d-4e57-bd96-d6aa05601ad8",
    "nodeDeallocationOption": "invalid",
    "currentDedicated": 4,
    "targetDedicated": 4,
    "enableAutoScale": false,
    "isAutoPool": false,
    "startTime": "2016-09-09T22:13:06.573Z",
    "endTime": "2016-09-09T22:14:01.727Z",
    "result": "Success",
    "resizeError": "The operation succeeded"
}
```

|<span data-ttu-id="cf3eb-106">Element</span><span class="sxs-lookup"><span data-stu-id="cf3eb-106">Element</span></span>|<span data-ttu-id="cf3eb-107">Typ</span><span class="sxs-lookup"><span data-stu-id="cf3eb-107">Type</span></span>|<span data-ttu-id="cf3eb-108">Poznámky</span><span class="sxs-lookup"><span data-stu-id="cf3eb-108">Notes</span></span>|
|-------------|----------|-----------|
|<span data-ttu-id="cf3eb-109">id</span><span class="sxs-lookup"><span data-stu-id="cf3eb-109">id</span></span>|<span data-ttu-id="cf3eb-110">Řetězec</span><span class="sxs-lookup"><span data-stu-id="cf3eb-110">String</span></span>|<span data-ttu-id="cf3eb-111">Id fondu.</span><span class="sxs-lookup"><span data-stu-id="cf3eb-111">The id of the pool.</span></span>|
|<span data-ttu-id="cf3eb-112">nodeDeallocationOption</span><span class="sxs-lookup"><span data-stu-id="cf3eb-112">nodeDeallocationOption</span></span>|<span data-ttu-id="cf3eb-113">Řetězec</span><span class="sxs-lookup"><span data-stu-id="cf3eb-113">String</span></span>|<span data-ttu-id="cf3eb-114">Určuje, kdy může odebrat uzly ve fondu, pokud zmenšování velikosti fondu.</span><span class="sxs-lookup"><span data-stu-id="cf3eb-114">Specifies when nodes may be removed from the pool, if the pool size is decreasing.</span></span><br /><br /> <span data-ttu-id="cf3eb-115">Možné hodnoty:</span><span class="sxs-lookup"><span data-stu-id="cf3eb-115">Possible values are:</span></span><br /><br /> <span data-ttu-id="cf3eb-116">**requeue** – ukončí se všechny spuštěné úlohy a znovu je.</span><span class="sxs-lookup"><span data-stu-id="cf3eb-116">**requeue** – Terminate running tasks and requeue them.</span></span> <span data-ttu-id="cf3eb-117">Úkoly se znovu spustí, když bude povolena úloha.</span><span class="sxs-lookup"><span data-stu-id="cf3eb-117">The tasks will run again when the job is enabled.</span></span> <span data-ttu-id="cf3eb-118">Odeberte uzly, jakmile úkoly ukončí.</span><span class="sxs-lookup"><span data-stu-id="cf3eb-118">Remove nodes as soon as tasks have been terminated.</span></span><br /><br /> <span data-ttu-id="cf3eb-119">**Ukončit** – ukončit spuštěné úkoly.</span><span class="sxs-lookup"><span data-stu-id="cf3eb-119">**terminate** – Terminate running tasks.</span></span> <span data-ttu-id="cf3eb-120">Úkoly se znovu nespustí.</span><span class="sxs-lookup"><span data-stu-id="cf3eb-120">The tasks will not run again.</span></span> <span data-ttu-id="cf3eb-121">Odeberte uzly, jakmile úkoly ukončí.</span><span class="sxs-lookup"><span data-stu-id="cf3eb-121">Remove nodes as soon as tasks have been terminated.</span></span><br /><br /> <span data-ttu-id="cf3eb-122">**taskcompletion** – povolit aktuálně spuštěných úloh k dokončení.</span><span class="sxs-lookup"><span data-stu-id="cf3eb-122">**taskcompletion** – Allow currently running tasks to complete.</span></span> <span data-ttu-id="cf3eb-123">Plánovat žádné nové úkoly při čekání.</span><span class="sxs-lookup"><span data-stu-id="cf3eb-123">Schedule no new tasks while waiting.</span></span> <span data-ttu-id="cf3eb-124">Odeberte uzly po dokončení všech úloh.</span><span class="sxs-lookup"><span data-stu-id="cf3eb-124">Remove nodes when all tasks have completed.</span></span><br /><br /> <span data-ttu-id="cf3eb-125">**Retaineddata** -povolit aktuálně spuštěných úkolů dokončily. pak počkejte, než všech úkolů období uchovávání dat vyprší.</span><span class="sxs-lookup"><span data-stu-id="cf3eb-125">**Retaineddata** -  Allow currently running tasks to complete, then wait for all task data retention periods to expire.</span></span> <span data-ttu-id="cf3eb-126">Plánovat žádné nové úkoly při čekání.</span><span class="sxs-lookup"><span data-stu-id="cf3eb-126">Schedule no new tasks while waiting.</span></span> <span data-ttu-id="cf3eb-127">Odeberte uzly, pokud vypršela platnost období uchovávání všech úloh.</span><span class="sxs-lookup"><span data-stu-id="cf3eb-127">Remove nodes when all task retention periods have expired.</span></span><br /><br /> <span data-ttu-id="cf3eb-128">Výchozí hodnota je requeue.</span><span class="sxs-lookup"><span data-stu-id="cf3eb-128">The default value is requeue.</span></span><br /><br /> <span data-ttu-id="cf3eb-129">Pokud je zvýšení velikosti fondu, pak je hodnota nastavená **neplatný**.</span><span class="sxs-lookup"><span data-stu-id="cf3eb-129">If the pool size is increasing then the value is set to **invalid**.</span></span>|
|<span data-ttu-id="cf3eb-130">currentDedicated</span><span class="sxs-lookup"><span data-stu-id="cf3eb-130">currentDedicated</span></span>|<span data-ttu-id="cf3eb-131">Int32</span><span class="sxs-lookup"><span data-stu-id="cf3eb-131">Int32</span></span>|<span data-ttu-id="cf3eb-132">Počet aktuálně přiřazená k fondu výpočetních uzlů.</span><span class="sxs-lookup"><span data-stu-id="cf3eb-132">The number of compute nodes currently assigned to the pool.</span></span>|
|<span data-ttu-id="cf3eb-133">targetDedicated</span><span class="sxs-lookup"><span data-stu-id="cf3eb-133">targetDedicated</span></span>|<span data-ttu-id="cf3eb-134">Int32</span><span class="sxs-lookup"><span data-stu-id="cf3eb-134">Int32</span></span>|<span data-ttu-id="cf3eb-135">Počet výpočetních uzlů, které jsou požadovány pro fond.</span><span class="sxs-lookup"><span data-stu-id="cf3eb-135">The number of compute nodes that are requested for the pool.</span></span>|
|<span data-ttu-id="cf3eb-136">enableAutoScale</span><span class="sxs-lookup"><span data-stu-id="cf3eb-136">enableAutoScale</span></span>|<span data-ttu-id="cf3eb-137">BOOL</span><span class="sxs-lookup"><span data-stu-id="cf3eb-137">Bool</span></span>|<span data-ttu-id="cf3eb-138">Určuje, zda velikost fondu automaticky přizpůsobí v čase.</span><span class="sxs-lookup"><span data-stu-id="cf3eb-138">Specifies whether the pool size automatically adjusts over time.</span></span>|
|<span data-ttu-id="cf3eb-139">isAutoPool</span><span class="sxs-lookup"><span data-stu-id="cf3eb-139">isAutoPool</span></span>|<span data-ttu-id="cf3eb-140">BOOL</span><span class="sxs-lookup"><span data-stu-id="cf3eb-140">Bool</span></span>|<span data-ttu-id="cf3eb-141">Určuje, zda fondu se vytvořil prostřednictvím mechanismu AutoPool úlohy.</span><span class="sxs-lookup"><span data-stu-id="cf3eb-141">Specifies whether the pool was created via a job's AutoPool mechanism.</span></span>|
|<span data-ttu-id="cf3eb-142">startTime</span><span class="sxs-lookup"><span data-stu-id="cf3eb-142">startTime</span></span>|<span data-ttu-id="cf3eb-143">Data a času</span><span class="sxs-lookup"><span data-stu-id="cf3eb-143">DateTime</span></span>|<span data-ttu-id="cf3eb-144">Změnit velikost fondu čas spuštění.</span><span class="sxs-lookup"><span data-stu-id="cf3eb-144">The time the pool resize started.</span></span>|
|<span data-ttu-id="cf3eb-145">čas ukončení</span><span class="sxs-lookup"><span data-stu-id="cf3eb-145">endTime</span></span>|<span data-ttu-id="cf3eb-146">Data a času</span><span class="sxs-lookup"><span data-stu-id="cf3eb-146">DateTime</span></span>|<span data-ttu-id="cf3eb-147">Čas změnit velikost fondu byla dokončena.</span><span class="sxs-lookup"><span data-stu-id="cf3eb-147">The time the pool resize completed.</span></span>|
|<span data-ttu-id="cf3eb-148">resultCode</span><span class="sxs-lookup"><span data-stu-id="cf3eb-148">resultCode</span></span>|<span data-ttu-id="cf3eb-149">Řetězec</span><span class="sxs-lookup"><span data-stu-id="cf3eb-149">String</span></span>|<span data-ttu-id="cf3eb-150">Výsledek změna.</span><span class="sxs-lookup"><span data-stu-id="cf3eb-150">The result of the resize.</span></span>|
|<span data-ttu-id="cf3eb-151">resultMessage</span><span class="sxs-lookup"><span data-stu-id="cf3eb-151">resultMessage</span></span>|<span data-ttu-id="cf3eb-152">Řetězec</span><span class="sxs-lookup"><span data-stu-id="cf3eb-152">String</span></span>|<span data-ttu-id="cf3eb-153">Chyba změny velikosti obsahuje podrobnosti výsledku.</span><span class="sxs-lookup"><span data-stu-id="cf3eb-153">The resize error includes the details of the result.</span></span><br /><br /> <span data-ttu-id="cf3eb-154">Pokud se změna úspěšně dokončit ho stavy, které operace byla úspěšná.</span><span class="sxs-lookup"><span data-stu-id="cf3eb-154">If the resize completed successfully it states that the operation succeeded.</span></span>|