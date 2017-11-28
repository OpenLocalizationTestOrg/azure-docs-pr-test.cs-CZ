---
title: "vstupy vzorkování AAA v Azure Stream Analytics | Microsoft Docs"
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
ms.openlocfilehash: 9637a8664de099eebb8f5654036d2957f4c6b7ce
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-stream-analytics-input-stream-sampling"></a><span data-ttu-id="d7893-104">Vzorkování vstupu stream Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="d7893-104">Azure Stream Analytics input-stream sampling</span></span>

<span data-ttu-id="d7893-105">Pomocí Azure Stream Analytics můžete zkusit vstupních událostech, které pocházejí ze souboru a testování dotazů hello portálu bez nutnosti toostart nebo zastavení úlohy.</span><span class="sxs-lookup"><span data-stu-id="d7893-105">By using Azure Stream Analytics, you can sample input events that come from a file and test queries in hello portal without needing toostart or stop a job.</span></span>

## <a name="testing-your-query"></a><span data-ttu-id="d7893-106">Testování dotazu</span><span class="sxs-lookup"><span data-stu-id="d7893-106">Testing your query</span></span>

<span data-ttu-id="d7893-107">V podokně podrobností úlohy Stream Analytics hello otevřete hello **editor dotazů** tak, že kliknete na název dotazu hello pod **dotazu**.</span><span class="sxs-lookup"><span data-stu-id="d7893-107">In hello Stream Analytics job details pane, open hello **Query editor** blade by clicking hello query name under **Query**.</span></span> <span data-ttu-id="d7893-108">(V našem ukázkový scénář, protože zatím nebyla vytvořena žádná dotazu, klikněte na tlačítko hello **< >** zástupný symbol.)</span><span class="sxs-lookup"><span data-stu-id="d7893-108">(In our example scenario, because no query has been created yet, click hello **< >** placeholder.)</span></span>

![editor dotazů Hello Stream Analytics](media/stream-analytics-sample-data-input/stream-analytics-query-editor.png)

<span data-ttu-id="d7893-110">stejně jako tomu bylo v předchozí verzi hello, zobrazí se okno Hello bohatě vybavený editor pro vytvoření dotazu.</span><span class="sxs-lookup"><span data-stu-id="d7893-110">hello rich editor blade for creating your query is displayed as it was in hello previous release.</span></span> <span data-ttu-id="d7893-111">Teď hello okno byl aktualizován na novou levé podokno, že zobrazuje hello vstupy a výstupy, které jsou používané hello dotazu a definované pro tuto úlohu.</span><span class="sxs-lookup"><span data-stu-id="d7893-111">Now hello blade has been updated with a new left pane that shows hello inputs and outputs that are used by hello query and defined for this job.</span></span>

![editor dotazů Stream Analytics Hello vstup a výstupy seznamy](media/stream-analytics-sample-data-input/stream-analytics-query-editor-highlight.png)

<span data-ttu-id="d7893-113">Rovněž jsou uvedeny další vstupu a výstupu, které nejsou definovány.</span><span class="sxs-lookup"><span data-stu-id="d7893-113">Also shown are an additional input and output, which are not defined.</span></span> <span data-ttu-id="d7893-114">Pocházejí ze hello novou dotazu šablonu, kterou spustíte s.</span><span class="sxs-lookup"><span data-stu-id="d7893-114">They come from hello new query template that you start with.</span></span> <span data-ttu-id="d7893-115">Změňte, nebo i zmizí, jak upravit dotaz hello.</span><span class="sxs-lookup"><span data-stu-id="d7893-115">They change, or even disappear altogether, as you edit hello query.</span></span> <span data-ttu-id="d7893-116">Můžete bez obav ignorovat je teď.</span><span class="sxs-lookup"><span data-stu-id="d7893-116">You can safely ignore them for now.</span></span>

<span data-ttu-id="d7893-117">Klikněte pravým tlačítkem na vstupy tootest s ukázkovými daty vstupní a potom vyberte **nahrát ukázková data ze souboru**.</span><span class="sxs-lookup"><span data-stu-id="d7893-117">tootest with sample input data, right-click any of your inputs, and then select **Upload sample data from file**.</span></span>

![Hello Stream Analytics dotazu editor nahrávání ukázková data z soubor – příkaz](media/stream-analytics-sample-data-input/stream-analytics-query-editor-upload.png)

<span data-ttu-id="d7893-119">Po dokončení nahrávání hello klikněte na tlačítko **Test** tootest tento dotaz hello vzorová data, které jste právě zadali.</span><span class="sxs-lookup"><span data-stu-id="d7893-119">After hello upload is complete, click **Test** tootest this query against hello sample data you have just provided.</span></span>

![tlačítko Testovat editor dotazů Stream Analytics Hello](media/stream-analytics-sample-data-input/stream-analytics-query-editor-test.png)

<span data-ttu-id="d7893-121">Pokud chcete výstup testu hello toosave pro pozdější použití, hello výstup tohoto dotazu se zobrazí v prohlížeči hello s výsledky stahování toohello a odkaz.</span><span class="sxs-lookup"><span data-stu-id="d7893-121">If you want toosave hello test output for later use, hello output of your query is displayed in hello browser with a link toohello download results.</span></span> <span data-ttu-id="d7893-122">Teď můžete snadno a interaktivně upravte dotaz a otestovat ji opakovaně toosee jak výstup hello změny.</span><span class="sxs-lookup"><span data-stu-id="d7893-122">You can now easily and iteratively modify your query and test it repeatedly toosee how hello output changes.</span></span>

![Stream Analytics query editor ukázkový výstup](media/stream-analytics-sample-data-input/stream-analytics-query-editor-samples-output.png)

<span data-ttu-id="d7893-124">V hello předcházející bitové kopie, byl přidán druhý výstup, nazývá **HighAvgTempOutput**.</span><span class="sxs-lookup"><span data-stu-id="d7893-124">In hello preceding image, a second output has been added, called **HighAvgTempOutput**.</span></span>

<span data-ttu-id="d7893-125">Použijete-li několik výstupů v dotazu, můžete zobrazit hello výsledky pro každý výstup samostatně a snadno přepínat mezi nimi.</span><span class="sxs-lookup"><span data-stu-id="d7893-125">When you use multiple outputs in a query, you can see hello results for each output separately and easily toggle between them.</span></span>

<span data-ttu-id="d7893-126">Jakmile budete spokojeni s výsledky hello, můžete uložit dotazu, spustit úlohu, sledujte a sledovat magické číslo hello Stream Analytics.</span><span class="sxs-lookup"><span data-stu-id="d7893-126">After you are satisfied with hello results, you can save your query, start your job, sit back and watch hello magic of Stream Analytics.</span></span>

## <a name="get-help"></a><span data-ttu-id="d7893-127">Podpora</span><span class="sxs-lookup"><span data-stu-id="d7893-127">Get help</span></span>

<span data-ttu-id="d7893-128">Pro další pomoc, vyzkoušejte naše [fórum Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span><span class="sxs-lookup"><span data-stu-id="d7893-128">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span></span>

## <a name="next-steps"></a><span data-ttu-id="d7893-129">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d7893-129">Next steps</span></span>
* [<span data-ttu-id="d7893-130">Úvod tooAzure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="d7893-130">Introduction tooAzure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="d7893-131">Začínáme používat službu Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="d7893-131">Get started using Azure Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="d7893-132">Škálování služby Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="d7893-132">Scale Azure Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="d7893-133">Referenční příručka k jazyku Azure Stream Analytics Query Language</span><span class="sxs-lookup"><span data-stu-id="d7893-133">Azure Stream Analytics Query Language Reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="d7893-134">Referenční příručka k rozhraní REST API pro správu služby Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="d7893-134">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)
