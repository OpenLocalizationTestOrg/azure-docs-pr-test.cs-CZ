---
title: "Vzorkování vstupy v Azure Stream Analytics | Microsoft Docs"
description: "Přesně stanovit problémy při řešení potíží s úlohy Stream Analytics."
keywords: "řešení potíží s vstupní, vstupní vzorkování"
documentationcenter: 
services: stream-analytics
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
ms.openlocfilehash: 0bb66090b5025d57f5ca8f30713aef4e444fa8e7
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="azure-stream-analytics-input-stream-sampling"></a><span data-ttu-id="11a76-104">Vzorkování vstupu stream Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="11a76-104">Azure Stream Analytics input-stream sampling</span></span>

<span data-ttu-id="11a76-105">Pomocí Azure Stream Analytics můžete zkusit vstupní události, které pocházejí ze souboru a testování dotazů na portálu bez nutnosti spuštění nebo zastavení úlohy.</span><span class="sxs-lookup"><span data-stu-id="11a76-105">By using Azure Stream Analytics, you can sample input events that come from a file and test queries in the portal without needing to start or stop a job.</span></span>

## <a name="testing-your-query"></a><span data-ttu-id="11a76-106">Testování dotazu</span><span class="sxs-lookup"><span data-stu-id="11a76-106">Testing your query</span></span>

<span data-ttu-id="11a76-107">V podokně podrobností úlohy Stream Analytics, otevřete **editor dotazů** tak, že kliknete na název dotazu v části **dotazu**.</span><span class="sxs-lookup"><span data-stu-id="11a76-107">In the Stream Analytics job details pane, open the **Query editor** blade by clicking the query name under **Query**.</span></span> <span data-ttu-id="11a76-108">(V našem ukázkový scénář, protože zatím nebyla vytvořena žádná dotazu, klikněte na tlačítko **< >** zástupný symbol.)</span><span class="sxs-lookup"><span data-stu-id="11a76-108">(In our example scenario, because no query has been created yet, click the **< >** placeholder.)</span></span>

![Editor dotazů Stream Analytics](media/stream-analytics-sample-data-input/stream-analytics-query-editor.png)

<span data-ttu-id="11a76-110">Stejně jako tomu bylo v předchozí verzi, zobrazí se okno bohatě vybavený editor pro vytvoření dotazu.</span><span class="sxs-lookup"><span data-stu-id="11a76-110">The rich editor blade for creating your query is displayed as it was in the previous release.</span></span> <span data-ttu-id="11a76-111">Teď byl aktualizován v okně s novou levém podokně, který ukazuje vstupy a výstupy, které se používají v dotazu a jsou definované pro tuto úlohu.</span><span class="sxs-lookup"><span data-stu-id="11a76-111">Now the blade has been updated with a new left pane that shows the inputs and outputs that are used by the query and defined for this job.</span></span>

![Editor dotazů Stream Analytics vstup a výstupy seznamy](media/stream-analytics-sample-data-input/stream-analytics-query-editor-highlight.png)

<span data-ttu-id="11a76-113">Rovněž jsou uvedeny další vstupu a výstupu, které nejsou definovány.</span><span class="sxs-lookup"><span data-stu-id="11a76-113">Also shown are an additional input and output, which are not defined.</span></span> <span data-ttu-id="11a76-114">Pocházejí ze novou šablonu dotazu, kterou spustíte s.</span><span class="sxs-lookup"><span data-stu-id="11a76-114">They come from the new query template that you start with.</span></span> <span data-ttu-id="11a76-115">Změňte, nebo i zmizí, jak upravit dotaz.</span><span class="sxs-lookup"><span data-stu-id="11a76-115">They change, or even disappear altogether, as you edit the query.</span></span> <span data-ttu-id="11a76-116">Můžete bez obav ignorovat je teď.</span><span class="sxs-lookup"><span data-stu-id="11a76-116">You can safely ignore them for now.</span></span>

<span data-ttu-id="11a76-117">Chcete-li otestovat s ukázkovými daty vstupní, klikněte pravým tlačítkem na vstupy a pak vyberte **nahrát ukázková data ze souboru**.</span><span class="sxs-lookup"><span data-stu-id="11a76-117">To test with sample input data, right-click any of your inputs, and then select **Upload sample data from file**.</span></span>

![Stream Analytics query editor odeslání ukázkových dat z soubor – příkaz](media/stream-analytics-sample-data-input/stream-analytics-query-editor-upload.png)

<span data-ttu-id="11a76-119">Po dokončení nahrávání se klikněte na tlačítko **testování** k testování tento dotaz ukázková data, které jste právě zadali.</span><span class="sxs-lookup"><span data-stu-id="11a76-119">After the upload is complete, click **Test** to test this query against the sample data you have just provided.</span></span>

![Tlačítko Testovat editor dotazů Stream Analytics](media/stream-analytics-sample-data-input/stream-analytics-query-editor-test.png)

<span data-ttu-id="11a76-121">Pokud chcete uložit test výstup pro pozdější použití, výstup tohoto dotazu se zobrazí v prohlížeči se zobrazí odkaz na výsledky stahování.</span><span class="sxs-lookup"><span data-stu-id="11a76-121">If you want to save the test output for later use, the output of your query is displayed in the browser with a link to the download results.</span></span> <span data-ttu-id="11a76-122">Teď můžete snadno a interaktivně upravte dotaz a otestovat opakovaně a najdete v části Jak výstup změny.</span><span class="sxs-lookup"><span data-stu-id="11a76-122">You can now easily and iteratively modify your query and test it repeatedly to see how the output changes.</span></span>

![Stream Analytics query editor ukázkový výstup](media/stream-analytics-sample-data-input/stream-analytics-query-editor-samples-output.png)

<span data-ttu-id="11a76-124">Na předchozím obrázku byl přidán druhý výstup, nazývá **HighAvgTempOutput**.</span><span class="sxs-lookup"><span data-stu-id="11a76-124">In the preceding image, a second output has been added, called **HighAvgTempOutput**.</span></span>

<span data-ttu-id="11a76-125">Použijete-li několik výstupů v dotazu, můžete zobrazit výsledky pro každý výstup samostatně a snadno přepínat mezi nimi.</span><span class="sxs-lookup"><span data-stu-id="11a76-125">When you use multiple outputs in a query, you can see the results for each output separately and easily toggle between them.</span></span>

<span data-ttu-id="11a76-126">Až budete s výsledky spokojeni, můžete uložit dotazu, spustit úlohu, sledujte a sledovat magické číslo Stream Analytics.</span><span class="sxs-lookup"><span data-stu-id="11a76-126">After you are satisfied with the results, you can save your query, start your job, sit back and watch the magic of Stream Analytics.</span></span>

## <a name="get-help"></a><span data-ttu-id="11a76-127">Podpora</span><span class="sxs-lookup"><span data-stu-id="11a76-127">Get help</span></span>

<span data-ttu-id="11a76-128">Pro další pomoc, vyzkoušejte naše [fórum Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span><span class="sxs-lookup"><span data-stu-id="11a76-128">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span></span>

## <a name="next-steps"></a><span data-ttu-id="11a76-129">Další kroky</span><span class="sxs-lookup"><span data-stu-id="11a76-129">Next steps</span></span>
* [<span data-ttu-id="11a76-130">Úvod do služby Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="11a76-130">Introduction to Azure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="11a76-131">Začínáme používat službu Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="11a76-131">Get started using Azure Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="11a76-132">Škálování služby Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="11a76-132">Scale Azure Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="11a76-133">Referenční příručka k jazyku Azure Stream Analytics Query Language</span><span class="sxs-lookup"><span data-stu-id="11a76-133">Azure Stream Analytics Query Language Reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="11a76-134">Referenční příručka k rozhraní REST API pro správu služby Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="11a76-134">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)
