---
title: "AAA \"počáteční událost změny velikosti fondu Azure Batch | Microsoft Docs\""
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
ms.openlocfilehash: 2ca2a4f1195c3f785ae5b051b63340f70eecbc22
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="pool-resize-start-event"></a><span data-ttu-id="ac383-103">Událost spuštění změny velikosti fondu</span><span class="sxs-lookup"><span data-stu-id="ac383-103">Pool resize start event</span></span>

 <span data-ttu-id="ac383-104">Tato událost je vygenerované při bylo zahájeno změny velikosti fondu.</span><span class="sxs-lookup"><span data-stu-id="ac383-104">This event is emitted when a pool resize has started.</span></span> <span data-ttu-id="ac383-105">Vzhledem k tomu, že změny velikosti fondu hello je asynchronní událostí, můžete očekávat toobe fondu změny velikosti událost complete vygenerované po hello změnit velikost operace dokončí.</span><span class="sxs-lookup"><span data-stu-id="ac383-105">Since hello pool resize is an asynchronous event, you can expect a pool resize complete event toobe emitted once hello resize operation completes.</span></span>

 <span data-ttu-id="ac383-106">změnit velikost Hello následující ukázka, že zobrazuje hello tělo události spuštění změny velikosti fondu pro změnu velikosti fondu z 0 too2 uzlů s ruční.</span><span class="sxs-lookup"><span data-stu-id="ac383-106">hello following example shows hello body of a pool resize start event for a pool resizing from 0 too2 nodes with a manual resize.</span></span>

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

|<span data-ttu-id="ac383-107">Element</span><span class="sxs-lookup"><span data-stu-id="ac383-107">Element</span></span>|<span data-ttu-id="ac383-108">Typ</span><span class="sxs-lookup"><span data-stu-id="ac383-108">Type</span></span>|<span data-ttu-id="ac383-109">Poznámky</span><span class="sxs-lookup"><span data-stu-id="ac383-109">Notes</span></span>|
|-------------|----------|-----------|
|<span data-ttu-id="ac383-110">poolId</span><span class="sxs-lookup"><span data-stu-id="ac383-110">poolId</span></span>|<span data-ttu-id="ac383-111">Řetězec</span><span class="sxs-lookup"><span data-stu-id="ac383-111">String</span></span>|<span data-ttu-id="ac383-112">Hello id fondu hello.</span><span class="sxs-lookup"><span data-stu-id="ac383-112">hello id of hello pool.</span></span>|
|<span data-ttu-id="ac383-113">nodeDeallocationOption</span><span class="sxs-lookup"><span data-stu-id="ac383-113">nodeDeallocationOption</span></span>|<span data-ttu-id="ac383-114">Řetězec</span><span class="sxs-lookup"><span data-stu-id="ac383-114">String</span></span>|<span data-ttu-id="ac383-115">Určuje, kdy může odebrat uzly hello fondu, pokud zmenšování velikosti fondu hello.</span><span class="sxs-lookup"><span data-stu-id="ac383-115">Specifies when nodes may be removed from hello pool, if hello pool size is decreasing.</span></span><br /><br /> <span data-ttu-id="ac383-116">Možné hodnoty:</span><span class="sxs-lookup"><span data-stu-id="ac383-116">Possible values are:</span></span><br /><br /> <span data-ttu-id="ac383-117">**requeue** – ukončí se všechny spuštěné úlohy a znovu je.</span><span class="sxs-lookup"><span data-stu-id="ac383-117">**requeue** – Terminate running tasks and requeue them.</span></span> <span data-ttu-id="ac383-118">Hello úkoly se znovu spustí, když je povolena úloha hello.</span><span class="sxs-lookup"><span data-stu-id="ac383-118">hello tasks will run again when hello job is enabled.</span></span> <span data-ttu-id="ac383-119">Odeberte uzly, jakmile úkoly ukončí.</span><span class="sxs-lookup"><span data-stu-id="ac383-119">Remove nodes as soon as tasks have been terminated.</span></span><br /><br /> <span data-ttu-id="ac383-120">**Ukončit** – ukončit spuštěné úkoly.</span><span class="sxs-lookup"><span data-stu-id="ac383-120">**terminate** – Terminate running tasks.</span></span> <span data-ttu-id="ac383-121">Hello úkoly se znovu nespustí.</span><span class="sxs-lookup"><span data-stu-id="ac383-121">hello tasks will not run again.</span></span> <span data-ttu-id="ac383-122">Odeberte uzly, jakmile úkoly ukončí.</span><span class="sxs-lookup"><span data-stu-id="ac383-122">Remove nodes as soon as tasks have been terminated.</span></span><br /><br /> <span data-ttu-id="ac383-123">**taskcompletion** – povolit aktuálně spuštěné úlohy toocomplete.</span><span class="sxs-lookup"><span data-stu-id="ac383-123">**taskcompletion** – Allow currently running tasks toocomplete.</span></span> <span data-ttu-id="ac383-124">Plánovat žádné nové úkoly při čekání.</span><span class="sxs-lookup"><span data-stu-id="ac383-124">Schedule no new tasks while waiting.</span></span> <span data-ttu-id="ac383-125">Odeberte uzly po dokončení všech úloh.</span><span class="sxs-lookup"><span data-stu-id="ac383-125">Remove nodes when all tasks have completed.</span></span><br /><br /> <span data-ttu-id="ac383-126">**Retaineddata** – povolit aktuálně spuštěné úlohy toocomplete a potom počkejte, než všech úkolů tooexpire období uchovávání dat.</span><span class="sxs-lookup"><span data-stu-id="ac383-126">**Retaineddata** - Allow currently running tasks toocomplete, then wait for all task data retention periods tooexpire.</span></span> <span data-ttu-id="ac383-127">Plánovat žádné nové úkoly při čekání.</span><span class="sxs-lookup"><span data-stu-id="ac383-127">Schedule no new tasks while waiting.</span></span> <span data-ttu-id="ac383-128">Odeberte uzly, pokud vypršela platnost období uchovávání všech úloh.</span><span class="sxs-lookup"><span data-stu-id="ac383-128">Remove nodes when all task retention periods have expired.</span></span><br /><br /> <span data-ttu-id="ac383-129">Hello výchozí hodnota je requeue.</span><span class="sxs-lookup"><span data-stu-id="ac383-129">hello default value is requeue.</span></span><br /><br /> <span data-ttu-id="ac383-130">Pokud je zvýšení velikosti fondu hello pak hello hodnota příliš**neplatný**.</span><span class="sxs-lookup"><span data-stu-id="ac383-130">If hello pool size is increasing then hello value is set too**invalid**.</span></span>|
|<span data-ttu-id="ac383-131">currentDedicated</span><span class="sxs-lookup"><span data-stu-id="ac383-131">currentDedicated</span></span>|<span data-ttu-id="ac383-132">Int32</span><span class="sxs-lookup"><span data-stu-id="ac383-132">Int32</span></span>|<span data-ttu-id="ac383-133">Hello počet výpočetních uzlů aktuálně přiřazená toohello fondu.</span><span class="sxs-lookup"><span data-stu-id="ac383-133">hello number of compute nodes currently assigned toohello pool.</span></span>|
|<span data-ttu-id="ac383-134">targetDedicated</span><span class="sxs-lookup"><span data-stu-id="ac383-134">targetDedicated</span></span>|<span data-ttu-id="ac383-135">Int32</span><span class="sxs-lookup"><span data-stu-id="ac383-135">Int32</span></span>|<span data-ttu-id="ac383-136">Hello počet výpočetních uzlů, které jsou požadovány pro fond hello.</span><span class="sxs-lookup"><span data-stu-id="ac383-136">hello number of compute nodes that are requested for hello pool.</span></span>|
|<span data-ttu-id="ac383-137">enableAutoScale</span><span class="sxs-lookup"><span data-stu-id="ac383-137">enableAutoScale</span></span>|<span data-ttu-id="ac383-138">BOOL</span><span class="sxs-lookup"><span data-stu-id="ac383-138">Bool</span></span>|<span data-ttu-id="ac383-139">Určuje, zda velikost fondu hello automaticky přizpůsobí v čase.</span><span class="sxs-lookup"><span data-stu-id="ac383-139">Specifies whether hello pool size automatically adjusts over time.</span></span>|
|<span data-ttu-id="ac383-140">isAutoPool</span><span class="sxs-lookup"><span data-stu-id="ac383-140">isAutoPool</span></span>|<span data-ttu-id="ac383-141">BOOL</span><span class="sxs-lookup"><span data-stu-id="ac383-141">Bool</span></span>|<span data-ttu-id="ac383-142">Speficies jestli hello fondu se vytvořil prostřednictvím mechanismu AutoPool úlohy.</span><span class="sxs-lookup"><span data-stu-id="ac383-142">Speficies whether hello pool was created via a job's AutoPool mechanism.</span></span>|
