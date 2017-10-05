---
title: "Postup spuštění streamování úlohy v Stream Analytics | Microsoft Docs"
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
ms.openlocfilehash: 9a3ff37a893b0f29a2ac2eda6cd50687ee779ead
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-run-a-streaming-job-in-azure-stream-analytics"></a><span data-ttu-id="f7e26-104">Jak spustit úlohu streamování v Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="f7e26-104">How to run a streaming job in Azure Stream Analytics</span></span>
<span data-ttu-id="f7e26-105">Když úloha vstup, dotaz a výstup všech nebyly zadány, že můžete spustit úlohu služby Stream Analytics.</span><span class="sxs-lookup"><span data-stu-id="f7e26-105">When a job input, query and output have all been specified you can start the Stream Analytics job.</span></span>

<span data-ttu-id="f7e26-106">Při spuštění vaší úlohy:</span><span class="sxs-lookup"><span data-stu-id="f7e26-106">To start your job:</span></span>

1. <span data-ttu-id="f7e26-107">Na portálu Azure Classic na řídicím panelu úloh, klikněte na tlačítko **spustit** v dolní části stránky.</span><span class="sxs-lookup"><span data-stu-id="f7e26-107">In the Azure Classic portal, from the job dashboard, click **Start** at the bottom of the page.</span></span>
   
   ![Spuštění úlohy tlačítko](./media/stream-analytics-run-a-job/1-stream-analytics-run-a-job.png)  
   
   <span data-ttu-id="f7e26-109">Na portálu Azure klikněte na tlačítko **spustit** v horní části stránku úlohy.</span><span class="sxs-lookup"><span data-stu-id="f7e26-109">In the Azure portal, click **Start** at the top of your job page.</span></span>
   
   ![Úloha spuštění Azure portálu tlačítko](./media/stream-analytics-run-a-job/4-stream-analytics-run-a-job.png)  
2. <span data-ttu-id="f7e26-111">Zadejte **spustit výstup** hodnotu, která určí, když tato úloha spustí vytváření výstupu.</span><span class="sxs-lookup"><span data-stu-id="f7e26-111">Specify a **Start Output** value to determine when this job will start producing output.</span></span> <span data-ttu-id="f7e26-112">Výchozí nastavení pro úlohy, které nebyly dříve spuštěna je **čas spuštění úlohy**, což znamená, že úloha okamžitě spustí zpracování dat.</span><span class="sxs-lookup"><span data-stu-id="f7e26-112">The default setting for jobs that have not previously been started is **Job Start Time**, which means that the job will immediately start processing data.</span></span> <span data-ttu-id="f7e26-113">Můžete také zadat **vlastní** čas v minulosti (pro použití historických dat) nebo později (a zdržet zpracování dokud datum v budoucnosti).</span><span class="sxs-lookup"><span data-stu-id="f7e26-113">You can also specify a **Custom** time in the past (for consuming historical data) or the future (to delay processing until a future time).</span></span> <span data-ttu-id="f7e26-114">Pro případech, kdy úloha má dříve spuštění a zastavení, možnost **naposledy Zastaveno** je k dispozici, aby bylo možné pokračovat v úloze při posledním výstup a nedošlo ke ztrátě dat..</span><span class="sxs-lookup"><span data-stu-id="f7e26-114">For cases when a job has been previously started and stopped, the option **Last Stopped Time** is available in order to resume the job from the last output time and avoid data loss.</span></span>  
   
   ![Počáteční čas úlohy streamování](./media/stream-analytics-run-a-job/2-stream-analytics-run-a-job.png)  
   
   ![Azure portálu spustit úlohu streamování čas](./media/stream-analytics-run-a-job/5-stream-analytics-run-a-job.png)  
3. <span data-ttu-id="f7e26-117">Potvrďte výběr.</span><span class="sxs-lookup"><span data-stu-id="f7e26-117">Confirm your selection.</span></span> <span data-ttu-id="f7e26-118">Stav úlohy se změní na *počáteční* a krátce přesune do *systémem* po úlohu spustil.</span><span class="sxs-lookup"><span data-stu-id="f7e26-118">The job status will change to *Starting* and will shortly move to *Running* once the job has started.</span></span> <span data-ttu-id="f7e26-119">Můžete sledovat průběh **spustit** operace v **centra oznámení**:</span><span class="sxs-lookup"><span data-stu-id="f7e26-119">You can monitor the progress of the **Start** operation in the **Notification Hub**:</span></span>
   
   ![průběh úlohy streamování](./media/stream-analytics-run-a-job/3-stream-analytics-run-a-job.png)  
   
   ![Portál Azure průběh úlohy streamování](./media/stream-analytics-run-a-job/6-stream-analytics-run-a-job.png)  

## <a name="get-help"></a><span data-ttu-id="f7e26-122">Podpora</span><span class="sxs-lookup"><span data-stu-id="f7e26-122">Get help</span></span>
<span data-ttu-id="f7e26-123">Další podporu naleznete v našem [fóru služby Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span><span class="sxs-lookup"><span data-stu-id="f7e26-123">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span></span>

## <a name="next-steps"></a><span data-ttu-id="f7e26-124">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f7e26-124">Next steps</span></span>
* [<span data-ttu-id="f7e26-125">Úvod do služby Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="f7e26-125">Introduction to Azure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="f7e26-126">Začínáme používat službu Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="f7e26-126">Get started using Azure Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="f7e26-127">Škálování služby Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="f7e26-127">Scale Azure Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="f7e26-128">Referenční příručka k jazyku Azure Stream Analytics Query Language</span><span class="sxs-lookup"><span data-stu-id="f7e26-128">Azure Stream Analytics Query Language Reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="f7e26-129">Referenční příručka k rozhraní REST API pro správu služby Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="f7e26-129">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)

