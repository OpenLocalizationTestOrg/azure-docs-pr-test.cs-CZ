---
title: "AAA \"fondu Azure Batch změnit velikost událost complete | Microsoft Docs\""
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
ms.openlocfilehash: dc64711a01aa4cf6192edba1a2c4cad56f953766
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="pool-resize-complete-event"></a><span data-ttu-id="8972a-103">Dokončení událost změny velikosti fondu</span><span class="sxs-lookup"><span data-stu-id="8972a-103">Pool resize complete event</span></span>

 <span data-ttu-id="8972a-104">Tato událost je vygenerované při změny velikosti fondu má byla dokončena nebo se nezdařilo.</span><span class="sxs-lookup"><span data-stu-id="8972a-104">This event is emitted when a pool resize has completed or failed.</span></span>

 <span data-ttu-id="8972a-105">Hello následující příklad ukazuje textu hello událost complete změny velikosti fondu pro skupinu, která tento nárůst velikosti a byla úspěšně dokončena.</span><span class="sxs-lookup"><span data-stu-id="8972a-105">hello following example shows hello body of a pool resize complete event for a pool that increased in size and completed successfully.</span></span>

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
    "resizeError": "hello operation succeeded"
}
```

|<span data-ttu-id="8972a-106">Element</span><span class="sxs-lookup"><span data-stu-id="8972a-106">Element</span></span>|<span data-ttu-id="8972a-107">Typ</span><span class="sxs-lookup"><span data-stu-id="8972a-107">Type</span></span>|<span data-ttu-id="8972a-108">Poznámky</span><span class="sxs-lookup"><span data-stu-id="8972a-108">Notes</span></span>|
|-------------|----------|-----------|
|<span data-ttu-id="8972a-109">id</span><span class="sxs-lookup"><span data-stu-id="8972a-109">id</span></span>|<span data-ttu-id="8972a-110">Řetězec</span><span class="sxs-lookup"><span data-stu-id="8972a-110">String</span></span>|<span data-ttu-id="8972a-111">Hello id fondu hello.</span><span class="sxs-lookup"><span data-stu-id="8972a-111">hello id of hello pool.</span></span>|
|<span data-ttu-id="8972a-112">nodeDeallocationOption</span><span class="sxs-lookup"><span data-stu-id="8972a-112">nodeDeallocationOption</span></span>|<span data-ttu-id="8972a-113">Řetězec</span><span class="sxs-lookup"><span data-stu-id="8972a-113">String</span></span>|<span data-ttu-id="8972a-114">Určuje, kdy může odebrat uzly hello fondu, pokud zmenšování velikosti fondu hello.</span><span class="sxs-lookup"><span data-stu-id="8972a-114">Specifies when nodes may be removed from hello pool, if hello pool size is decreasing.</span></span><br /><br /> <span data-ttu-id="8972a-115">Možné hodnoty:</span><span class="sxs-lookup"><span data-stu-id="8972a-115">Possible values are:</span></span><br /><br /> <span data-ttu-id="8972a-116">**requeue** – ukončí se všechny spuštěné úlohy a znovu je.</span><span class="sxs-lookup"><span data-stu-id="8972a-116">**requeue** – Terminate running tasks and requeue them.</span></span> <span data-ttu-id="8972a-117">Hello úkoly se znovu spustí, když je povolena úloha hello.</span><span class="sxs-lookup"><span data-stu-id="8972a-117">hello tasks will run again when hello job is enabled.</span></span> <span data-ttu-id="8972a-118">Odeberte uzly, jakmile úkoly ukončí.</span><span class="sxs-lookup"><span data-stu-id="8972a-118">Remove nodes as soon as tasks have been terminated.</span></span><br /><br /> <span data-ttu-id="8972a-119">**Ukončit** – ukončit spuštěné úkoly.</span><span class="sxs-lookup"><span data-stu-id="8972a-119">**terminate** – Terminate running tasks.</span></span> <span data-ttu-id="8972a-120">Hello úkoly se znovu nespustí.</span><span class="sxs-lookup"><span data-stu-id="8972a-120">hello tasks will not run again.</span></span> <span data-ttu-id="8972a-121">Odeberte uzly, jakmile úkoly ukončí.</span><span class="sxs-lookup"><span data-stu-id="8972a-121">Remove nodes as soon as tasks have been terminated.</span></span><br /><br /> <span data-ttu-id="8972a-122">**taskcompletion** – povolit aktuálně spuštěné úlohy toocomplete.</span><span class="sxs-lookup"><span data-stu-id="8972a-122">**taskcompletion** – Allow currently running tasks toocomplete.</span></span> <span data-ttu-id="8972a-123">Plánovat žádné nové úkoly při čekání.</span><span class="sxs-lookup"><span data-stu-id="8972a-123">Schedule no new tasks while waiting.</span></span> <span data-ttu-id="8972a-124">Odeberte uzly po dokončení všech úloh.</span><span class="sxs-lookup"><span data-stu-id="8972a-124">Remove nodes when all tasks have completed.</span></span><br /><br /> <span data-ttu-id="8972a-125">**Retaineddata** – povolit aktuálně spuštěné úlohy toocomplete a potom počkejte, než všech úkolů tooexpire období uchovávání dat.</span><span class="sxs-lookup"><span data-stu-id="8972a-125">**Retaineddata** -  Allow currently running tasks toocomplete, then wait for all task data retention periods tooexpire.</span></span> <span data-ttu-id="8972a-126">Plánovat žádné nové úkoly při čekání.</span><span class="sxs-lookup"><span data-stu-id="8972a-126">Schedule no new tasks while waiting.</span></span> <span data-ttu-id="8972a-127">Odeberte uzly, pokud vypršela platnost období uchovávání všech úloh.</span><span class="sxs-lookup"><span data-stu-id="8972a-127">Remove nodes when all task retention periods have expired.</span></span><br /><br /> <span data-ttu-id="8972a-128">Hello výchozí hodnota je requeue.</span><span class="sxs-lookup"><span data-stu-id="8972a-128">hello default value is requeue.</span></span><br /><br /> <span data-ttu-id="8972a-129">Pokud je zvýšení velikosti fondu hello pak hello hodnota příliš**neplatný**.</span><span class="sxs-lookup"><span data-stu-id="8972a-129">If hello pool size is increasing then hello value is set too**invalid**.</span></span>|
|<span data-ttu-id="8972a-130">currentDedicated</span><span class="sxs-lookup"><span data-stu-id="8972a-130">currentDedicated</span></span>|<span data-ttu-id="8972a-131">Int32</span><span class="sxs-lookup"><span data-stu-id="8972a-131">Int32</span></span>|<span data-ttu-id="8972a-132">Hello počet výpočetních uzlů aktuálně přiřazená toohello fondu.</span><span class="sxs-lookup"><span data-stu-id="8972a-132">hello number of compute nodes currently assigned toohello pool.</span></span>|
|<span data-ttu-id="8972a-133">targetDedicated</span><span class="sxs-lookup"><span data-stu-id="8972a-133">targetDedicated</span></span>|<span data-ttu-id="8972a-134">Int32</span><span class="sxs-lookup"><span data-stu-id="8972a-134">Int32</span></span>|<span data-ttu-id="8972a-135">Hello počet výpočetních uzlů, které jsou požadovány pro fond hello.</span><span class="sxs-lookup"><span data-stu-id="8972a-135">hello number of compute nodes that are requested for hello pool.</span></span>|
|<span data-ttu-id="8972a-136">enableAutoScale</span><span class="sxs-lookup"><span data-stu-id="8972a-136">enableAutoScale</span></span>|<span data-ttu-id="8972a-137">BOOL</span><span class="sxs-lookup"><span data-stu-id="8972a-137">Bool</span></span>|<span data-ttu-id="8972a-138">Určuje, zda velikost fondu hello automaticky přizpůsobí v čase.</span><span class="sxs-lookup"><span data-stu-id="8972a-138">Specifies whether hello pool size automatically adjusts over time.</span></span>|
|<span data-ttu-id="8972a-139">isAutoPool</span><span class="sxs-lookup"><span data-stu-id="8972a-139">isAutoPool</span></span>|<span data-ttu-id="8972a-140">BOOL</span><span class="sxs-lookup"><span data-stu-id="8972a-140">Bool</span></span>|<span data-ttu-id="8972a-141">Určuje, zda hello fondu se vytvořil prostřednictvím úlohy AutoPool mechanismus.</span><span class="sxs-lookup"><span data-stu-id="8972a-141">Specifies whether hello pool was created via a job's AutoPool mechanism.</span></span>|
|<span data-ttu-id="8972a-142">startTime</span><span class="sxs-lookup"><span data-stu-id="8972a-142">startTime</span></span>|<span data-ttu-id="8972a-143">Data a času</span><span class="sxs-lookup"><span data-stu-id="8972a-143">DateTime</span></span>|<span data-ttu-id="8972a-144">Dobrý den, když velikost fondu hello spustit.</span><span class="sxs-lookup"><span data-stu-id="8972a-144">hello time hello pool resize started.</span></span>|
|<span data-ttu-id="8972a-145">endTime</span><span class="sxs-lookup"><span data-stu-id="8972a-145">endTime</span></span>|<span data-ttu-id="8972a-146">Data a času</span><span class="sxs-lookup"><span data-stu-id="8972a-146">DateTime</span></span>|<span data-ttu-id="8972a-147">Dobrý den, když velikost fondu hello dokončit.</span><span class="sxs-lookup"><span data-stu-id="8972a-147">hello time hello pool resize completed.</span></span>|
|<span data-ttu-id="8972a-148">resultCode</span><span class="sxs-lookup"><span data-stu-id="8972a-148">resultCode</span></span>|<span data-ttu-id="8972a-149">Řetězec</span><span class="sxs-lookup"><span data-stu-id="8972a-149">String</span></span>|<span data-ttu-id="8972a-150">změnit velikost Hello výsledek hello.</span><span class="sxs-lookup"><span data-stu-id="8972a-150">hello result of hello resize.</span></span>|
|<span data-ttu-id="8972a-151">resultMessage</span><span class="sxs-lookup"><span data-stu-id="8972a-151">resultMessage</span></span>|<span data-ttu-id="8972a-152">Řetězec</span><span class="sxs-lookup"><span data-stu-id="8972a-152">String</span></span>|<span data-ttu-id="8972a-153">Chyba změny velikosti Hello obsahuje hello podrobnosti výsledku hello.</span><span class="sxs-lookup"><span data-stu-id="8972a-153">hello resize error includes hello details of hello result.</span></span><br /><br /> <span data-ttu-id="8972a-154">Pokud velikost hello byla úspěšně dokončena ho stavy, které hello operace byla úspěšná.</span><span class="sxs-lookup"><span data-stu-id="8972a-154">If hello resize completed successfully it states that hello operation succeeded.</span></span>|
