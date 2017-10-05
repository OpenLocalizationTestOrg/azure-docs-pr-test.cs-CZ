---
title: "Ladění se příjemců u centra událostí Azure Stream Analytics | Microsoft Docs"
description: "Osvědčené postupy pro Event Hubs skupiny uživatelů v úlohy Stream Analytics využívá dotaz."
keywords: "limit centra událostí, skupiny uživatelů"
services: stream-analytics
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 04/20/2017
ms.author: jeffstok
ms.openlocfilehash: 145981d0b5eff0c574c5012c85f43a6318ba4126
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="debug-azure-stream-analytics-with-event-hub-receivers"></a><span data-ttu-id="ad0e5-104">Ladění Azure Stream Analytics se příjemců u centra událostí</span><span class="sxs-lookup"><span data-stu-id="ad0e5-104">Debug Azure Stream Analytics with event hub receivers</span></span>

<span data-ttu-id="ad0e5-105">Azure Event Hubs můžete v Azure Stream Analytics ingestování nebo výstupní data z úlohy.</span><span class="sxs-lookup"><span data-stu-id="ad0e5-105">You can use Azure Event Hubs in Azure Stream Analytics to ingest or output data from a job.</span></span> <span data-ttu-id="ad0e5-106">Doporučený postup pro použití služby Event Hubs je použití více skupin příjemce zajistit škálovatelnost úlohy.</span><span class="sxs-lookup"><span data-stu-id="ad0e5-106">A best practice for using Event Hubs is to use multiple consumer groups, to ensure job scalability.</span></span> <span data-ttu-id="ad0e5-107">Jedním z důvodů je, že počet čtenářů v úloze Stream Analytics pro specifický vstup ovlivní počet čtenářů v skupinu jednoho příjemce.</span><span class="sxs-lookup"><span data-stu-id="ad0e5-107">One reason is that the number of readers in the Stream Analytics job for a specific input affects the number of readers in a single consumer group.</span></span> <span data-ttu-id="ad0e5-108">Přesné počet příjemců vychází podrobnosti interní implementace pro logiku topologie Škálováním na více systémů.</span><span class="sxs-lookup"><span data-stu-id="ad0e5-108">The precise number of receivers is based on internal implementation details for the scale-out topology logic.</span></span> <span data-ttu-id="ad0e5-109">Počet příjemců nebude vystavena, externě.</span><span class="sxs-lookup"><span data-stu-id="ad0e5-109">The number of receivers is not exposed externally.</span></span> <span data-ttu-id="ad0e5-110">Při spuštění úlohy nebo během upgradů úlohy, můžete změnit počet čtenářů.</span><span class="sxs-lookup"><span data-stu-id="ad0e5-110">The number of readers can change either at the job start time or during job upgrades.</span></span>

> [!NOTE]
> <span data-ttu-id="ad0e5-111">Při změně počet čtenářů během upgradu úlohy přechodný upozornění se zapisují do protokolů auditů.</span><span class="sxs-lookup"><span data-stu-id="ad0e5-111">When the number of readers changes during a job upgrade, transient warnings are written to audit logs.</span></span> <span data-ttu-id="ad0e5-112">Úlohy Stream Analytics automaticky zotavit tyto přechodné problémy.</span><span class="sxs-lookup"><span data-stu-id="ad0e5-112">Stream Analytics jobs automatically recover from these transient issues.</span></span>

## <a name="number-of-readers-per-partition-exceeds-event-hubs-limit-of-five"></a><span data-ttu-id="ad0e5-113">Počet čtenářů na oddíl překračuje limit služby Event Hubs pět</span><span class="sxs-lookup"><span data-stu-id="ad0e5-113">Number of readers per partition exceeds Event Hubs limit of five</span></span>

<span data-ttu-id="ad0e5-114">Scénáře, ve kterých počet čtenářů na oddíl překračuje limit služby Event Hubs pět zahrnují následující:</span><span class="sxs-lookup"><span data-stu-id="ad0e5-114">Scenarios in which the number of readers per partition exceeds the Event Hubs limit of five include the following:</span></span>

* <span data-ttu-id="ad0e5-115">Více příkazů SELECT: Pokud používáte více příkazů SELECT, které odkazují na **stejné** vstupní centra událostí, každý příkaz SELECT způsobí, že nový příjemce, který se má vytvořit.</span><span class="sxs-lookup"><span data-stu-id="ad0e5-115">Multiple SELECT statements: If you use multiple SELECT statements that refer to **same** event hub input, each SELECT statement causes a new receiver to be created.</span></span>
* <span data-ttu-id="ad0e5-116">SJEDNOCENÍ: Při použití spojení, je možné, že více vstupů, které odkazují na **stejné** skupina rozbočovače a příjemce událostí.</span><span class="sxs-lookup"><span data-stu-id="ad0e5-116">UNION: When you use a UNION, it's possible to have multiple inputs that refer to the **same** event hub and consumer group.</span></span>
* <span data-ttu-id="ad0e5-117">SPOJENÍ sama: Při použití připojení k SAMOOBSLUŽNÉ operace, je možné odkazovat **stejné** centra událostí vícekrát.</span><span class="sxs-lookup"><span data-stu-id="ad0e5-117">SELF JOIN: When you use a SELF JOIN operation, it's possible to refer to the **same** event hub multiple times.</span></span>

## <a name="solution"></a><span data-ttu-id="ad0e5-118">Řešení</span><span class="sxs-lookup"><span data-stu-id="ad0e5-118">Solution</span></span>

<span data-ttu-id="ad0e5-119">Následující osvědčené postupy může pomoci zmírnit scénáře, ve kterých počet čtenářů na oddíl překračuje limit služby Event Hubs pět.</span><span class="sxs-lookup"><span data-stu-id="ad0e5-119">The following best practices can help mitigate scenarios in which the number of readers per partition exceeds the Event Hubs limit of five.</span></span>

### <a name="split-your-query-into-multiple-steps-by-using-a-with-clause"></a><span data-ttu-id="ad0e5-120">Rozdělením více kroků dotazu pomocí klauzule WITH</span><span class="sxs-lookup"><span data-stu-id="ad0e5-120">Split your query into multiple steps by using a WITH clause</span></span>

<span data-ttu-id="ad0e5-121">V klauzuli WITH určuje je sada výsledků dotazu dočasné s názvem, který lze odkazovat pomocí klauzule FROM v dotazu.</span><span class="sxs-lookup"><span data-stu-id="ad0e5-121">The WITH clause specifies a temporary named result set that can be referenced by a FROM clause in the query.</span></span> <span data-ttu-id="ad0e5-122">V klauzuli WITH definujete v oboru provedení jednoho příkazu SELECT.</span><span class="sxs-lookup"><span data-stu-id="ad0e5-122">You define the WITH clause in the execution scope of a single SELECT statement.</span></span>

<span data-ttu-id="ad0e5-123">Například místo tento dotaz:</span><span class="sxs-lookup"><span data-stu-id="ad0e5-123">For example, instead of this query:</span></span>

```
SELECT foo 
INTO output1
FROM inputEventHub

SELECT bar
INTO output2
FROM inputEventHub 
…
```

<span data-ttu-id="ad0e5-124">Pomocí tohoto dotazu:</span><span class="sxs-lookup"><span data-stu-id="ad0e5-124">Use this query:</span></span>

```
WITH input (
   SELECT * FROM inputEventHub
) as data

SELECT foo
INTO output1
FROM data

SELECT bar
INTO output2
FROM data
…
```

### <a name="ensure-that-inputs-bind-to-different-consumer-groups"></a><span data-ttu-id="ad0e5-125">Ujistěte se, že vstupy vázat na skupiny různých uživatelů</span><span class="sxs-lookup"><span data-stu-id="ad0e5-125">Ensure that inputs bind to different consumer groups</span></span>

<span data-ttu-id="ad0e5-126">Pro dotazy, v nichž jsou propojeny tři nebo více vstupů do stejné skupiny příjemců Event Hubs vytvořte skupiny samostatné příjemce.</span><span class="sxs-lookup"><span data-stu-id="ad0e5-126">For queries in which three or more inputs are connected to the same Event Hubs consumer group, create separate consumer groups.</span></span> <span data-ttu-id="ad0e5-127">To vyžaduje vytvoření další Stream Analytics vstupy.</span><span class="sxs-lookup"><span data-stu-id="ad0e5-127">This requires the creation of additional Stream Analytics inputs.</span></span>


## <a name="get-help"></a><span data-ttu-id="ad0e5-128">Podpora</span><span class="sxs-lookup"><span data-stu-id="ad0e5-128">Get help</span></span>
<span data-ttu-id="ad0e5-129">Pro další pomoc, vyzkoušejte naše [fórum Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span><span class="sxs-lookup"><span data-stu-id="ad0e5-129">For additional assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span></span>

## <a name="next-steps"></a><span data-ttu-id="ad0e5-130">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ad0e5-130">Next steps</span></span>
* [<span data-ttu-id="ad0e5-131">Úvod do služby Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="ad0e5-131">Introduction to Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="ad0e5-132">Začínáme s Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="ad0e5-132">Get started with Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="ad0e5-133">Škálování úlohy Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="ad0e5-133">Scale Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="ad0e5-134">Referenční příručka jazyka dotazů Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="ad0e5-134">Stream Analytics query language reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="ad0e5-135">Stream Analytics správu odkazu k REST API</span><span class="sxs-lookup"><span data-stu-id="ad0e5-135">Stream Analytics management REST API reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)
