---
title: "aaaHow toostart streamování úloh v Stream Analytics | Microsoft Docs"
description: "Jak spustit úlohu streamování v Azure Stream Analytics | učení segmentu cesty."
keywords: "Úloha streamování"
documentationcenter: 
services: stream-analytics
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 9d46950f-2b69-49ce-a567-df558c5dd820
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: jeffstok
ms.openlocfilehash: 67aa14860c38cbd0535d0ec4f23729445d0185c0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toorun-a-streaming-job-in-azure-stream-analytics"></a><span data-ttu-id="611ae-104">Jak toorun streamování úlohy Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="611ae-104">How toorun a streaming job in Azure Stream Analytics</span></span>
<span data-ttu-id="611ae-105">Při vstupu úlohy, dotaz a výstup všech nebyly zadány, že můžete spustit úlohy služby Stream Analytics hello.</span><span class="sxs-lookup"><span data-stu-id="611ae-105">When a job input, query and output have all been specified you can start hello Stream Analytics job.</span></span>

<span data-ttu-id="611ae-106">toostart vaše úlohy:</span><span class="sxs-lookup"><span data-stu-id="611ae-106">toostart your job:</span></span>

1. <span data-ttu-id="611ae-107">V portálu Azure Classic hello z řídicího panelu úloha hello, klikněte na **spustit** v hello dolní části stránky hello.</span><span class="sxs-lookup"><span data-stu-id="611ae-107">In hello Azure Classic portal, from hello job dashboard, click **Start** at hello bottom of hello page.</span></span>
   
   ![Spuštění úlohy tlačítko](./media/stream-analytics-run-a-job/1-stream-analytics-run-a-job.png)  
   
   <span data-ttu-id="611ae-109">V hello portálu Azure, klikněte na **spustit** v horní části hello stránky úlohy.</span><span class="sxs-lookup"><span data-stu-id="611ae-109">In hello Azure portal, click **Start** at hello top of your job page.</span></span>
   
   ![Úloha spuštění Azure portálu tlačítko](./media/stream-analytics-run-a-job/4-stream-analytics-run-a-job.png)  
2. <span data-ttu-id="611ae-111">Zadejte **spustit výstup** toodetermine hodnotu, pokud se tato úloha spustí vytváření výstupu.</span><span class="sxs-lookup"><span data-stu-id="611ae-111">Specify a **Start Output** value toodetermine when this job will start producing output.</span></span> <span data-ttu-id="611ae-112">Hello výchozí nastavení pro úlohy, které nebyly dříve spuštěna je **čas spuštění úlohy**, což znamená, že úlohy hello okamžitě začne zpracování dat.</span><span class="sxs-lookup"><span data-stu-id="611ae-112">hello default setting for jobs that have not previously been started is **Job Start Time**, which means that hello job will immediately start processing data.</span></span> <span data-ttu-id="611ae-113">Můžete také zadat **vlastní** čas v hello minulosti (pro použití historických dat) nebo budoucí hello (toodelay zpracování dokud datum v budoucnosti).</span><span class="sxs-lookup"><span data-stu-id="611ae-113">You can also specify a **Custom** time in hello past (for consuming historical data) or hello future (toodelay processing until a future time).</span></span> <span data-ttu-id="611ae-114">Pro možnost hello případech, kdy úloha má dříve spuštění a zastavení, **naposledy Zastaveno** je k dispozici v pořadí tooresume hello úlohu z hello čas poslední výstup a nedošlo ke ztrátě dat..</span><span class="sxs-lookup"><span data-stu-id="611ae-114">For cases when a job has been previously started and stopped, hello option **Last Stopped Time** is available in order tooresume hello job from hello last output time and avoid data loss.</span></span>  
   
   ![Počáteční čas úlohy streamování](./media/stream-analytics-run-a-job/2-stream-analytics-run-a-job.png)  
   
   ![Azure portálu spustit úlohu streamování čas](./media/stream-analytics-run-a-job/5-stream-analytics-run-a-job.png)  
3. <span data-ttu-id="611ae-117">Potvrďte výběr.</span><span class="sxs-lookup"><span data-stu-id="611ae-117">Confirm your selection.</span></span> <span data-ttu-id="611ae-118">Stav úlohy Hello změní příliš*počáteční* a krátce přesune příliš*systémem* po zahájení úlohy hello.</span><span class="sxs-lookup"><span data-stu-id="611ae-118">hello job status will change too*Starting* and will shortly move too*Running* once hello job has started.</span></span> <span data-ttu-id="611ae-119">Můžete sledovat průběh hello hello **spustit** operace v hello **centra oznámení**:</span><span class="sxs-lookup"><span data-stu-id="611ae-119">You can monitor hello progress of hello **Start** operation in hello **Notification Hub**:</span></span>
   
   ![průběh úlohy streamování](./media/stream-analytics-run-a-job/3-stream-analytics-run-a-job.png)  
   
   ![Portál Azure průběh úlohy streamování](./media/stream-analytics-run-a-job/6-stream-analytics-run-a-job.png)  

## <a name="get-help"></a><span data-ttu-id="611ae-122">Podpora</span><span class="sxs-lookup"><span data-stu-id="611ae-122">Get help</span></span>
<span data-ttu-id="611ae-123">Další podporu naleznete v našem [fóru služby Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span><span class="sxs-lookup"><span data-stu-id="611ae-123">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span></span>

## <a name="next-steps"></a><span data-ttu-id="611ae-124">Další kroky</span><span class="sxs-lookup"><span data-stu-id="611ae-124">Next steps</span></span>
* [<span data-ttu-id="611ae-125">Úvod tooAzure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="611ae-125">Introduction tooAzure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="611ae-126">Začínáme používat službu Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="611ae-126">Get started using Azure Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="611ae-127">Škálování služby Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="611ae-127">Scale Azure Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="611ae-128">Referenční příručka k jazyku Azure Stream Analytics Query Language</span><span class="sxs-lookup"><span data-stu-id="611ae-128">Azure Stream Analytics Query Language Reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="611ae-129">Referenční příručka k rozhraní REST API pro správu služby Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="611ae-129">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)

