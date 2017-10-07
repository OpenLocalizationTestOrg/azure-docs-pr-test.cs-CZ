---
title: "aaaDebug Azure Stream Analytics s příjemců u centra událostí | Microsoft Docs"
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
ms.openlocfilehash: 89821e6273151de43b5e42d907e547c939e24824
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="debug-azure-stream-analytics-with-event-hub-receivers"></a><span data-ttu-id="12c30-104">Ladění Azure Stream Analytics se příjemců u centra událostí</span><span class="sxs-lookup"><span data-stu-id="12c30-104">Debug Azure Stream Analytics with event hub receivers</span></span>

<span data-ttu-id="12c30-105">Azure Event Hubs můžete použít v Azure Stream Analytics tooingest nebo výstupní data z úlohy.</span><span class="sxs-lookup"><span data-stu-id="12c30-105">You can use Azure Event Hubs in Azure Stream Analytics tooingest or output data from a job.</span></span> <span data-ttu-id="12c30-106">Doporučený postup pro použití služby Event Hubs je toouse více skupiny příjemců, škálovatelnost tooensure úlohy.</span><span class="sxs-lookup"><span data-stu-id="12c30-106">A best practice for using Event Hubs is toouse multiple consumer groups, tooensure job scalability.</span></span> <span data-ttu-id="12c30-107">Jedním z důvodů je, že hello počet čtenářů v úloze Stream Analytics hello pro specifický vstup ovlivňuje hello počet čtenářů v skupinu jednoho příjemce.</span><span class="sxs-lookup"><span data-stu-id="12c30-107">One reason is that hello number of readers in hello Stream Analytics job for a specific input affects hello number of readers in a single consumer group.</span></span> <span data-ttu-id="12c30-108">Hello přesné počet příjemců je založen na interní implementace podrobnosti pro logiku hello topologie Škálováním na více systémů.</span><span class="sxs-lookup"><span data-stu-id="12c30-108">hello precise number of receivers is based on internal implementation details for hello scale-out topology logic.</span></span> <span data-ttu-id="12c30-109">Hello počet příjemců nebude vystavena, externě.</span><span class="sxs-lookup"><span data-stu-id="12c30-109">hello number of receivers is not exposed externally.</span></span> <span data-ttu-id="12c30-110">Hello počet čtenářů, můžete změnit na čas spuštění úlohy hello nebo během upgradů úlohy.</span><span class="sxs-lookup"><span data-stu-id="12c30-110">hello number of readers can change either at hello job start time or during job upgrades.</span></span>

> [!NOTE]
> <span data-ttu-id="12c30-111">Při změně hello počet čtenářů během upgradu úlohy se zapisují protokoly tooaudit přechodný upozornění.</span><span class="sxs-lookup"><span data-stu-id="12c30-111">When hello number of readers changes during a job upgrade, transient warnings are written tooaudit logs.</span></span> <span data-ttu-id="12c30-112">Úlohy Stream Analytics automaticky zotavit tyto přechodné problémy.</span><span class="sxs-lookup"><span data-stu-id="12c30-112">Stream Analytics jobs automatically recover from these transient issues.</span></span>

## <a name="number-of-readers-per-partition-exceeds-event-hubs-limit-of-five"></a><span data-ttu-id="12c30-113">Počet čtenářů na oddíl překračuje limit služby Event Hubs pět</span><span class="sxs-lookup"><span data-stu-id="12c30-113">Number of readers per partition exceeds Event Hubs limit of five</span></span>

<span data-ttu-id="12c30-114">Scénáře, ve které hello počet čtenářů na oddíl překračuje limit služby Event Hubs hello pět zahrnout hello následující:</span><span class="sxs-lookup"><span data-stu-id="12c30-114">Scenarios in which hello number of readers per partition exceeds hello Event Hubs limit of five include hello following:</span></span>

* <span data-ttu-id="12c30-115">Více příkazů SELECT: Pokud používáte více příkazů SELECT, které odkazují příliš**stejné** vstupní centra událostí, každý příkaz SELECT způsobí, že nový toobe příjemce vytvořili.</span><span class="sxs-lookup"><span data-stu-id="12c30-115">Multiple SELECT statements: If you use multiple SELECT statements that refer too**same** event hub input, each SELECT statement causes a new receiver toobe created.</span></span>
* <span data-ttu-id="12c30-116">SJEDNOCENÍ: Pokud používáte spojení, je možné toohave více vstupů, které odkazují toohello **stejné** skupina rozbočovače a příjemce událostí.</span><span class="sxs-lookup"><span data-stu-id="12c30-116">UNION: When you use a UNION, it's possible toohave multiple inputs that refer toohello **same** event hub and consumer group.</span></span>
* <span data-ttu-id="12c30-117">SPOJENÍ sama: Pokud použijete operaci SAMOOBSLUŽNÉ připojení, je možné toorefer toohello **stejné** centra událostí vícekrát.</span><span class="sxs-lookup"><span data-stu-id="12c30-117">SELF JOIN: When you use a SELF JOIN operation, it's possible toorefer toohello **same** event hub multiple times.</span></span>

## <a name="solution"></a><span data-ttu-id="12c30-118">Řešení</span><span class="sxs-lookup"><span data-stu-id="12c30-118">Solution</span></span>

<span data-ttu-id="12c30-119">Hello následující osvědčené postupy může pomoci zmírnit scénáře, ve které hello počet čtenářů na oddíl překračuje limit služby Event Hubs hello pět.</span><span class="sxs-lookup"><span data-stu-id="12c30-119">hello following best practices can help mitigate scenarios in which hello number of readers per partition exceeds hello Event Hubs limit of five.</span></span>

### <a name="split-your-query-into-multiple-steps-by-using-a-with-clause"></a><span data-ttu-id="12c30-120">Rozdělením více kroků dotazu pomocí klauzule WITH</span><span class="sxs-lookup"><span data-stu-id="12c30-120">Split your query into multiple steps by using a WITH clause</span></span>

<span data-ttu-id="12c30-121">klauzule WITH Hello určuje je sada výsledků dotazu dočasné s názvem, který lze odkazovat pomocí klauzule FROM v dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="12c30-121">hello WITH clause specifies a temporary named result set that can be referenced by a FROM clause in hello query.</span></span> <span data-ttu-id="12c30-122">Klauzule WITH hello definujete v oboru provádění hello jednoho příkazu SELECT.</span><span class="sxs-lookup"><span data-stu-id="12c30-122">You define hello WITH clause in hello execution scope of a single SELECT statement.</span></span>

<span data-ttu-id="12c30-123">Například místo tento dotaz:</span><span class="sxs-lookup"><span data-stu-id="12c30-123">For example, instead of this query:</span></span>

```
SELECT foo 
INTO output1
FROM inputEventHub

SELECT bar
INTO output2
FROM inputEventHub 
…
```

<span data-ttu-id="12c30-124">Pomocí tohoto dotazu:</span><span class="sxs-lookup"><span data-stu-id="12c30-124">Use this query:</span></span>

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

### <a name="ensure-that-inputs-bind-toodifferent-consumer-groups"></a><span data-ttu-id="12c30-125">Ujistěte se, že vstupy vázat toodifferent skupiny uživatelů</span><span class="sxs-lookup"><span data-stu-id="12c30-125">Ensure that inputs bind toodifferent consumer groups</span></span>

<span data-ttu-id="12c30-126">Pro dotazy, ve kterých jsou tři nebo více vstupů připojené toohello stejné služby Event Hubs příjemce skupiny, vytvořit skupiny samostatné příjemců.</span><span class="sxs-lookup"><span data-stu-id="12c30-126">For queries in which three or more inputs are connected toohello same Event Hubs consumer group, create separate consumer groups.</span></span> <span data-ttu-id="12c30-127">To vyžaduje hello vytvoření další Stream Analytics vstupy.</span><span class="sxs-lookup"><span data-stu-id="12c30-127">This requires hello creation of additional Stream Analytics inputs.</span></span>


## <a name="get-help"></a><span data-ttu-id="12c30-128">Podpora</span><span class="sxs-lookup"><span data-stu-id="12c30-128">Get help</span></span>
<span data-ttu-id="12c30-129">Pro další pomoc, vyzkoušejte naše [fórum Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span><span class="sxs-lookup"><span data-stu-id="12c30-129">For additional assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span></span>

## <a name="next-steps"></a><span data-ttu-id="12c30-130">Další kroky</span><span class="sxs-lookup"><span data-stu-id="12c30-130">Next steps</span></span>
* [<span data-ttu-id="12c30-131">Úvod tooStream Analytics</span><span class="sxs-lookup"><span data-stu-id="12c30-131">Introduction tooStream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="12c30-132">Začínáme s Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="12c30-132">Get started with Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="12c30-133">Škálování úlohy Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="12c30-133">Scale Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="12c30-134">Referenční příručka jazyka dotazů Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="12c30-134">Stream Analytics query language reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="12c30-135">Stream Analytics správu odkazu k REST API</span><span class="sxs-lookup"><span data-stu-id="12c30-135">Stream Analytics management REST API reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)
